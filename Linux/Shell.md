----

```shell
#!/bin/bash
```


----

Simple command:

```shell
touch hello.sh
vim hello.sh
```

Inside hello.sh
```Shell
#!/bin/bash
echo "hello word"
```

Back to command line:
```Shell
bash hello.sh
```

---


Complex commands:

```shell
touch hello.sh
vim batch.sh
```

Inside:
```shell
#!/bin/bash
cd /home/hao/
touch batchScript.txt
echo "the art of command line" >> batchScript.txt
```

Back:
```shell
bash batch.sh
```

---

variable

```shell
A=2
echo $A
unset A

readonly B=3 #cannot unset, exist untill restart
echo $B

c=1+1
echo $c

d = "test test test"
echo $d
```

global variable:
```shell
export D 
```
