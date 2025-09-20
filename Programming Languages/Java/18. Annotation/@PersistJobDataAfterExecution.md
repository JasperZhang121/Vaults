
`@PersistJobDataAfterExecution` is a Quartz annotation you put on a **Job class** to tell Quartz: _after_ a job run finishes **successfully** (i.e., `execute()` returns without throwing an exception), **persist the current contents of that Job’s `JobDataMap` back into the `JobDetail`**. On the next run, your job sees the updated values.

### Why and when to use it

- You want to keep **lightweight state** (e.g., a counter, last processed ID, last run timestamp) across executions.
    
- You **don’t** want to introduce an external store just to remember this state.
    
- You’re okay with the constraint that **only successful runs** persist updates.
    

> For anything large, complex, or shared across many jobs/services, use a database/cache instead.

---

### How it works (exact semantics)

#### Success-only write-back

- Quartz will re-store the `JobDetail` (including its `JobDataMap`) **only if** `execute()` completes normally.
    
- If `execute()` throws **any** exception (including `JobExecutionException`), that run’s changes **won’t** be persisted.
    

#### Which map is actually persisted?

- **Persisted:** `context.getJobDetail().getJobDataMap()`
    
- **Not persisted by this annotation:** `context.getMergedJobDataMap()` (read-only combined view of Job + Trigger maps), and the Trigger’s own `JobDataMap`.
    

> So: **read** from the merged map if you like, but **write** to the JobDetail’s map if you want changes to stick.

#### Scope (per JobDetail, not per class)

- The annotation is on the class, but the state you persist attaches to a **specific `JobDetail` instance** (identified by its `JobKey`). Different `JobDetail`s using the same class keep **separate** state.
    

---

### Concurrency: always pair with `@DisallowConcurrentExecution`

If multiple instances of the **same `JobDetail`** run concurrently, they can race and overwrite each other’s `JobDataMap`. To avoid lost updates:

```java
@PersistJobDataAfterExecution
@DisallowConcurrentExecution
public class MyStatefulJob implements Job { ... }
```

`@DisallowConcurrentExecution` prevents parallel executions of the same `JobKey`.

---

### Storage backends and persistence across restarts

- **RAMJobStore:** State is in-memory. Your counter/timestamp persists across runs in the same process, but **disappears on scheduler restart**.
    
- **JDBCJobStore:** State is saved to the DB. Your `JobDataMap` survives **process restarts**.
    
    - Keep values **small and simple** (primitives, strings). Complex object serialization can cause compatibility issues.
        

---

### Minimal example: a persistent counter job

```java
import org.quartz.*;
import org.quartz.impl.StdSchedulerFactory;

import static org.quartz.JobBuilder.newJob;
import static org.quartz.TriggerBuilder.newTrigger;
import static org.quartz.SimpleScheduleBuilder.simpleSchedule;

@PersistJobDataAfterExecution
@DisallowConcurrentExecution
public class CounterJob implements Job {
    public static final String COUNT = "count";

    @Override
    public void execute(JobExecutionContext context) throws JobExecutionException {
        // Only changes to JobDetail's map will be persisted
        JobDataMap jobMap = context.getJobDetail().getJobDataMap();
        int n = jobMap.getInt(COUNT);
        n++;
        jobMap.put(COUNT, n);
        System.out.println("Run #" + n);

        // If you threw an exception here, this increment would NOT be persisted.
    }

    public static void main(String[] args) throws Exception {
        Scheduler scheduler = StdSchedulerFactory.getDefaultScheduler();

        JobDetail job = newJob(CounterJob.class)
                .withIdentity("counter", "demo")
                .usingJobData(COUNT, 0)   // initial state
                .build();

        Trigger trigger = newTrigger()
                .withIdentity("t1", "demo")
                .startNow()
                .withSchedule(simpleSchedule()
                        .withIntervalInSeconds(5)
                        .repeatForever())
                .build();

        scheduler.scheduleJob(job, trigger);
        scheduler.start();
    }
}
```

**Expected behavior:** Each execution prints a higher “Run #n”.

- With **RAMJobStore**, the count persists while the scheduler is running.
    
- With **JDBCJobStore**, it continues even after restarting the JVM.
    

---

### Practical patterns

#### Pattern 1: last-run timestamp

```java
public static final String LAST_RUN_AT = "lastRunAt";
JobDataMap map = context.getJobDetail().getJobDataMap();
long last = map.getLong(LAST_RUN_AT);   // 0 if missing
// ... do work since `last` ...
map.put(LAST_RUN_AT, System.currentTimeMillis());  // persisted on success
```

#### Pattern 2: incremental offset / paging cursor

Store a numeric offset or page token. On success, move it forward. On failure, throw; the offset stays unchanged and you’ll retry from the same place next run.

---

### Common pitfalls (and quick fixes)

1. **Updating the merged map** (`getMergedJobDataMap()`) and expecting persistence  
    → Write to `context.getJobDetail().getJobDataMap()` instead.
    
2. **Forgetting `@DisallowConcurrentExecution`**  
    → Race conditions and lost updates. Add the annotation when you persist state.
    
3. **Throwing exceptions but expecting state to save**  
    → No save on failure. Catch/handle if appropriate, or accept that failures don’t advance state.
    
4. **Putting large/complex objects into `JobDataMap` (JDBCJobStore)**  
    → Use primitives/strings or store IDs and look up full objects from your DB.
    
5. **Assuming class-wide shared state**  
    → State is per `JobDetail` (per `JobKey`). Multiple `JobDetail`s using the same class don’t share `JobDataMap`.
    

---

### Testing checklist

- Log the value you update each run to verify it increments/persists.
    
- Kill and restart the app:
    
    - If on **JDBCJobStore**, the value should continue from the last run.
        
    - If on **RAMJobStore**, it resets (as expected).
        
- Temporarily throw an exception in `execute()` and confirm the state does **not** advance on that run.
    

---

### FAQ

**Q: Does it persist Trigger `JobDataMap` changes?**  
A: No. Only the **JobDetail’s** `JobDataMap` is persisted by this annotation.

**Q: Can I use it without `@DisallowConcurrentExecution`?**  
A: You _can_, but you risk clobbering your state under concurrent runs. Use both together for stateful jobs.

**Q: Is this the same as the old `StatefulJob`?**  
A: In Quartz 2.x, the old approach was replaced by using **both** `@PersistJobDataAfterExecution` (persist state) **and** `@DisallowConcurrentExecution` (no parallel runs).

---

### One-sentence takeaway

> Update the **JobDetail’s `JobDataMap`** inside `execute()`; if the run **succeeds**, Quartz **writes it back**—and to keep that state safe, **disable concurrent executions** for the same `JobDetail`.