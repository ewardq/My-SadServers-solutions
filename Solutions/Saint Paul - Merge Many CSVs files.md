# Scenario: "Saint Paul": Merge Many CSVs files
**Type**: Do
**Description**: Join (merge) all the 338 files in /home/admin/polldayregistrations_enregistjourduscrutin?????.csv into one single /home/admin/all.csv file with the contents of all the CSV files in any order. There should be only one line with the names of the columns as a header.

**Test**: The "Check My Solution" button runs the script /home/admin/agent/check.sh, which you can see and execute.

---
## Notes and solution
To merge of of the CSV files it might be tempting to just conccatenate all of their contents together in the **all.csv** solution file

 ```bash
cat *.csv > all.csv
```
but this leads to headers being repeated all thorough the resulting file.

So, to fix this, instead of simply concatenating all of the files, it's necessary to concatenate only the common header (from the first file) 
 ```bash
head -n 1 file1.csv > combined.out && tail -n+2 -q *.csv >> combined.out
```

 ```bash
head -n 1 polldayregistrations_enregistjourduscrutin10001.csv > all.csv && tail -n+2 -q *.csv >> all.csv
```