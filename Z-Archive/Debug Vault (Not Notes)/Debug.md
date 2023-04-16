
Problem:

```
The file will have its original line endings in your working directory fatal: loose object 3ed62a9bcfa06739fa22f3bbc32cab02e19ccd34 (stored in .git/objects/3e/d62a9bcfa06739fa22f3bbc32cab02e19ccd34) is corrupt
```


Try:
```
git fsck --full
```

output:
```
error: unable to mmap .git/objects/3e/d62a9bcfa06739fa22f3bbc32cab02e19ccd34: Invalid argument error: 3ed62a9bcfa06739fa22f3bbc32cab02e19ccd34: object corrupt or missing: .git/objects/3e/d62a9bcfa06739fa22f3bbc32cab02e19ccd34 Checking object directories: 100% (256/256), done. dangling blob 7de00ff01b013cdc3c285b43a6c035a0685cce1e dangling blob 60e27eec7f2a63c2604a77e4eb38eb7d656bcf10 dangling blob 5e93b22b4e103e2ca3586eaa0fb312bdb339e19b dangling blob f143b74e9d15053434ebde8b3c99ed3a6f964edc dangling tree 0c9499ea0e10e7fdc1db61031759c62b93118897 dangling blob 9bb4d3f0e5333fc5ca958028cdd77791e7a6d735 missing blob 3ed62a9bcfa06739fa22f3bbc32cab02e19ccd34 dangling blob 25771e6e6f149ebbf49ddce8b29ffdefb91e3e09 dangling blob 237ab222eaa0bbc3cb0f7175ca88e04f5f7e0455 dangling blob 9a5a2aaad0d0c4de0568d0a5b36fd296289a5cd1 dangling blob 532b5ffef32c77355f4fa9b307361f4c7fad2159 dangling blob 8afc04a917faa3916561c6d9d0a87bee19bd7309 dangling blob 06dd7e139b8715094b4927c231fbb80878be8921 dangling blob 7f5f9840438e630051afe6b3ae1f9ded93485978
```

Try:
```
git gc --prune=now
```

Output:

```
  
Enumerating objects: 1836, done. Counting objects: 100% (1836/1836), done. Delta compression using up to 12 threads Compressing objects: 100% (1757/1757), done. fatal: unable to read 3ed62a9bcfa06739fa22f3bbc32cab02e19ccd34 fatal: failed to run repack
```

----

Solve by :
- clone repository again from remote origin
