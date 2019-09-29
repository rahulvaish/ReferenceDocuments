### Experimenting HDFS Basic Operations on Ubuntu-18
<hr>

##### List HDFS Folders: </br>
```
hdfs dfs -ls /
```

##### Create one HDFS Folder: </br>
```
 hdfs dfs -mkdir /hdfsfolder
```

##### Put a file in HDFS Folder: </br>
```
 hdfs dfs -put /home/rahul/MyFolder/NormalFile.txt /hdfsfolder
```

##### List the files in HDFS Folder: </br>
```
 hdfs dfs -ls /hdfsfolder
```
  - ##### Referring to Local File System *[justFile.txt is present in Local File System /home/rahul/JustFolder/]*:
```
rahul@rahul:~$ hdfs dfs -ls file:///home/rahul/JustFolder/
Found 1 items
-rw-r--r--   1 root root         10 2019-09-29 08:57 file:///home/rahul/JustFolder/justFile.txt
```
 - ##### Referring to HDFS *[JustFolder is not present/moved on HDFS]*:
```
rahul@rahul:~$ hdfs dfs -ls /home/rahul/JustFolder/
ls: `/home/rahul/JustFolder/': No such file or directory
rahul@rahul:~$
```

##### View the file in HDFS Folder: </br>
```
 hdfs dfs -cat /hdfsfolder/NormalFile.txt
```

##### Remove a file from HDFS Folder: </br>
```
 hdfs dfs -rm  /hdfsfolder/NormalFile.txt
```

##### Remove all files from HDFS Folder: </br>
```
 hdfs dfs -rm -r  /hdfsfolder/*
```

##### Remove HDFS Folder: </br>
```
 hdfs dfs -rmdir /hdfsfolder
```

##### Copy from LFS to HDFS Folder: </br>
```
hdfs dfs -cp file:///home/rahul/JustFolder/justFile.txt /hdfsfolder
FYI: hdfs dfs -cp /home/rahul/JustFolder/justFile.txt /hdfsfolder this does not works!!
```
<hr>

##### Append a file presnt in LFS to the file in HDFS: </br>
```
hdfs dfs -appendToFile file:///home/rahul/JustFolder/justFile.txt /hdfsfolder/justFile.txt
```


##### We can also navigate out HDFS and its contents here: </br>
![image](https://user-images.githubusercontent.com/45539698/59725059-e1331500-9249-11e9-8175-277c6bf898f0.png)

<hr>
