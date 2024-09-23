---
dg-publish: true
---
---
#bash #cut #wc
**Type:** Do
**Description:** There's a file _/home/admin/scores.txt_ with two columns (imagine the first number is a counter and the second one is a test score for example).  
  
Find the average (more precisely; the arithmetic mean: sum of numbers divided by how many numbers are there) of the numbers in the second column (find the average score).  
  
Use exactly two digits to the right of the decimal point. i. e., **use exactly two "decimal digits" without any rounding**. Eg: if average = 21.349 , the solution is 21.34. If average = 33.1 , the solution is 33.10.  
  
Save the solution in the _/home/admin/solution_ file, for example: echo "123.45" > ~/solution  
  
Tip: There's bc, Python3, Golang and sqlite3 installed in this VM.

---
### Notes and solution:
First, we print the file to see what we are working with.
![[Pasted image 20240817173943.png]]


We see that the second column contains the data to compute, so we `cut` that column.

```bash
cat /home/admin/scores.txt | cut --delimiter " " --fields 2
```
![[Pasted image 20240817174122.png]]
where we use a space _" "_ as a `delimiter` to indicate when a column ends, then, we select the second one with `--fields 2`.


After that, to sum all those rows, we first create a single string with _+_ as a separator (using `paste`) so we can pass that output to the command `bc` to interpret and sum all the numbers.

```bash
cat /home/admin/scores.txt | cut --delimiter " " --fields 2 | paste -sd+ -
```
![[Pasted image 20240817174804.png]]
where the `-s` (serial) option makes _paste_ display the lines on one file and the option `-d+` makes the command join them all with a _+_ sign.


```bash
cat /home/admin/scores.txt | cut --delimiter " " --fields 2 | paste -sd+ - | bc
```
![[Pasted image 20240817174603.png]]
we then pass that input to be interpreted by `bc` so it can be summed together.


Now that we have the sum of all numbers, we also need the total numbers so we can preform the arithmetic  mean.

We can get the total number with the command `wc -l` to count lines.
```bash
cat /home/admin/scores.txt | wc -l
```
![[Pasted image 20240817175828.png]]


or, since we are given a numbered list, we can also use the last line of the first column.

```bash
cat /home/admin/scores.txt | cut --delimiter " " --fields 1 | tail -n 1
```
![[Pasted image 20240817180053.png]]

To get the average, well use the command `bc` with a precision of 2 decimals (`scale=2`) and configured so it uses a float format (`-l`).

```bash
cat /home/admin/scores.txt | cut --delimiter " " --fields 2 | paste -sd+ - | bc | xargs -I % sh -c "echo 'scale=2; %/100'" | bc -l
```
![[Pasted image 20240817180351.png]]

With this, we conclude that the answer is ___5.20___.


