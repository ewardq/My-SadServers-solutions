---
dg-publish: true
---

# "Minneapolis": Break a CSV file
**Type:** Do
**Description:** Break the Comma Separated Valued (CSV) file _data.csv_ in the _/home/admin/_ directory into exactly 10 smaller files of about the same size named _data-00.csv_, _data-01.csv_, ... , _data-09.csv_ files in the same directory. All the files should have the same header (first line with column names) as _data.csv_. None of the smaller files should be bigger than 32KB.  
  
Note: to simplify, disregard broken lines in your files (ie, you can break a file at any point, not just at a newline). The resulting files don't have to be proper CSV files.

**Test:** The "Check My Solution" button runs the script _/home/admin/agent/check.sh_, which you can see and execute.

---
