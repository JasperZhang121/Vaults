`@DisallowConcurrentExecution` is a **class-level** Quartz annotation that tells the scheduler: **do not execute two instances of the same `JobDetail` (same `JobKey`) at the same time**. If a trigger fires while the job is still running, Quartz will hold that firing (which may become a **misfire**) and handle it according to the trigger’s misfire policy.

---

#### Why and when to use it

- Your job **reads/writes shared state** (e.g., a counter in `JobDataMap`, files, DB rows) and must be serialized to avoid races.
    
- You’re using `@PersistJobDataAfterExecution` to carry state between runs—pair them to avoid clobbering that state.
    
- The job performs **non-reentrant work** (e.g., touching the same resource path or row set).
    

> For throughput-oriented jobs that are safe to run in parallel, **don’t** use this annotation.

---

#### Scope & granularity (super important)

- **Per `JobDetail`**: Concurrency is blocked only for the same `JobKey` (name + group).
    
- **Not per class**: Two different `JobDetail`s using the same Job class **can** run concurrently.
    
- **Multiple triggers / same JobDetail**: Still serialized; if a second trigger fires while one run is active, it won’t start another run.
    

If you need **global** serialization across multiple `JobDetail`s, you must enforce it yourself (e.g., DB locks, distributed locks like Redis/Redisson, or a single shared `JobDetail`).

---

#### Behavior with triggers & misfires

- While a run is in progress, any firing for the same `JobDetail` cannot start immediately.
    
- If the delay exceeds the trigger’s tolerance window, Quartz marks it a **misfire** and applies the trigger’s **misfire instruction**:
    
    - **SimpleTrigger**: e.g., “Fire now and proceed,” or “Reschedule next with remaining count.”
        
    - **CronTrigger**: e.g., “Do nothing (skip missed fires)” or “Fire once now.”
        
- Choose a misfire policy that matches your semantics (catch up vs. skip).
    

---

#### Cluster behavior

- With **JDBCJobStore + clustering enabled**, “no concurrent execution” is enforced **cluster-wide**. Quartz coordinates through the DB, so the same `JobDetail` won’t run on two nodes simultaneously.
    

---

#### Interaction with `@PersistJobDataAfterExecution`

These two annotations often **go together**:

- `@PersistJobDataAfterExecution` saves updates to the **JobDetail’s** `JobDataMap` at the end of a **successful** run.
    
- `@DisallowConcurrentExecution` prevents overlapping runs from racing and overwriting that state.
    

This combination replaces the legacy `StatefulJob` pattern from Quartz 1.x.

---

#### Minimal example: single-threaded file ingester

```java
import org.quartz.*;
import org.quartz.impl.StdSchedulerFactory;

import static org.quartz.JobBuilder.newJob;
import static org.quartz.TriggerBuilder.newTrigger;
import static org.quartz.SimpleScheduleBuilder.simpleSchedule;

@DisallowConcurrentExecution
@PersistJobDataAfterExecution
public class FileIngestJob implements Job {
    public static final String LAST_FILE_ID = "lastFileId";

    @Override
    public void execute(JobExecutionContext ctx) throws JobExecutionException {
        JobDataMap map = ctx.getJobDetail().getJobDataMap();
        long last = map.getLong(LAST_FILE_ID);  // persisted cursor
        long next = processNextBatchStartingFrom(last); // your logic
        map.put(LAST_FILE_ID, next);            // persisted on success
    }

    private long processNextBatchStartingFrom(long last) {
        // read files > last, ingest, return new cursor
        return last + 10;
    }

    public static void main(String[] args) throws Exception {
        Scheduler scheduler = StdSchedulerFactory.getDefaultScheduler();

        JobDetail job = newJob(FileIngestJob.class)
                .withIdentity("ingest", "files")
                .usingJobData(LAST_FILE_ID, 0L)
                .build();

        Trigger t1 = newTrigger()
                .withIdentity("every10s", "files")
                .startNow()
                .withSchedule(simpleSchedule()
                        .withIntervalInSeconds(10)
                        .repeatForever())
                .build();

        scheduler.scheduleJob(job, t1);
        scheduler.start();
    }
}
```

**What you get**

- No overlapping runs for `JobKey("ingest","files")`.
    
- If a run takes >10s, subsequent firings wait (or misfire, per policy).
    
- The cursor (`LAST_FILE_ID`) updates safely.
    

---

#### Pitfalls & gotchas

1. **Expecting class-wide serialization**
    
    - Not provided. It’s per `JobDetail`. Use one shared `JobDetail` or an external lock if you need broader guarantees.
        
2. **Forgetting misfire policy**
    
    - Long-running jobs + frequent schedules can produce lots of misfires. Set explicit misfire instructions for your trigger.
        
3. **Assuming thread pool size is irrelevant**
    
    - The pool controls how many jobs _could_ run; this annotation only prevents _the same JobDetail_ from overlapping. Others can still consume threads.
        
4. **Pairing with `getMergedJobDataMap()` updates**
    
    - Only updates to **JobDetail’s** `JobDataMap` persist with `@PersistJobDataAfterExecution`. Don’t update the merged map and expect persistence.
        
5. **Sharding expectations**
    
    - If you need parallelism, create multiple **distinct `JobDetail`s** with partitioned inputs, or use a distributed queue/worker model.
        

---

#### Testing checklist

- Schedule a short interval and make the job intentionally slow—confirm only one run is active at a time.
    
- Add metrics/logging of `jobKey`, start/end timestamps, and any cursor/state changes.
    
- Simulate node A + node B in cluster mode; ensure no overlapping runs occur across nodes.
    
- Configure and verify the trigger’s misfire instruction (e.g., by stopping the scheduler, letting time pass, and restarting).
    

---

#### FAQ

**Q: Does it block different `JobDetail`s that use the same class?**  
A: No. It only serializes per `JobKey`.

**Q: What happens if a trigger fires while the job is running?**  
A: It cannot start a new run; depending on timing, it may become a misfire and follow the trigger’s misfire policy.

**Q: Do I still need a small thread pool if I use this?**  
A: This annotation doesn’t limit other jobs. Keep the pool sized for your overall workload.

---

### One-sentence takeaway

> `@DisallowConcurrentExecution` ensures **no overlapping runs for the same `JobDetail`**, which is essential for stateful or non-reentrant jobs—pair it with `@PersistJobDataAfterExecution` when you need safe, incremental state across executions.