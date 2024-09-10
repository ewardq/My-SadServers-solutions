---
dg-publish: true
---

# "Minneapolis": Break a CSV file
**Type:** Do
**Description:** Break the Comma Separated Valued (CSV) file _data.csv_ in the _/home/admin/_ directory into exactly 10 smaller files of about the same size named _data-00.csv_, _data-01.csv_, ... , _data-09.csv_ files in the same directory. All the files should have the same header (first line with column names) as _data.csv_. None of the smaller files should be bigger than 32KB.  
  
>Note
>To simplify, disregard broken lines in your files (ie, you can break a file at any point, not just at a newline). The resulting files don't have to be proper CSV files.

**Test:** The "Check My Solution" button runs the script _/home/admin/agent/check.sh_, which you can see and execute.

**OS:** Debian 11

---
## Notes and solution
First, let's find where is the file that will be processed.

```bash
ls /home/admin/
```
![[Pasted image 20240910113847.png]]
And because the current directory is the target directory, it's not necessary to change anything on that regard.

To split the file into 10 pieces, the `split` command is a built-in Linux command that offers almost all necessary features to solve the scenario.

```bash
split -d --number=10 --additional-suffix=.csv data.csv data-
```
where:
- `-d` tells the command to use numeric **suffixes** (01, 02, 03, ...) instead of letters.
- `--number=10` tells the command to split the _data.csv_ file into 10 parts.
- `--additional-suffix=.csv` makes it so the **suffix** _.csv_ is added to all the new files.
- The `data-` string is the **prefix** for the new files.

Now there are 10 newly created files based on the original _data.csv_ file.
```bash
ls
```
![[Pasted image 20240910114138.png]]

But only the _data-00.csv_ has the original file's headers. To fix this, select all the created files except the first one, add the first line of the _data-00.csv_ plus the rest of each file and save them.

```bash
for i in $(find . -type f -name "data-*.csv" -not -name "data-00.csv")
	do echo -e "$(head -1 data-00.csv)\n$(cat $i)" > $i;
done
```

Now, all the files have the original file's header.
![[Pasted image 20240910114836.png]]

___Success!___

