---
dg-publish: true
---

# "Santiago": Find the secret combination
**Level:** Easy
**Type:** Do
**Tags:** [bash](https://sadservers.com/tag/bash)  
**Description:** Alice the spy has hidden a secret number combination, find it using these instructions:  
  
1) Find the number of **lines** with occurrences of the string **Alice** (case sensitive) in the _*.txt_ files in the _/home/admin_ directory  
2) There's a file where **Alice** appears exactly once. In that file, in the line after that "Alice" occurrence there's a number.  
Write both numbers consecutively as one (no new line or spaces) to the solution file. For example if the first number from 1) is _11_ and the second _22_, you can do `echo -n 11 > /home/admin/solution; echo 22 >> /home/admin/solution` or `echo "1122" > /home/admin/solution`.

**Test:** Running `md5sum /home/admin/solution` returns d80e026d18a57b56bddf1d99a8a491f9(just a way to verify the solution without giving it away.)

---
### Notes and solution:
First, we list all the files on the _/home/admin/_ directory.
`ls /home/admin/`
![[Pasted image 20240817131656.png]]


There are one directory and one file that shouldn't be taken into consideration when counting the number of times **Alice** appears, so we filter those two with `find`.

```bash
find /home/admin/ -maxdepth 1 -iname "*.txt"
```
![[Pasted image 20240817131914.png]]
where `-maxdepth 1` ensures that we only search in the current directory (therefore filtering any files that may be in the unwanted sub directory) and `-iname "*.txt"` ensures we only see text files.

Now, we have to search for the word **Alice** in the files and count how many times it appears. Both of these things can be accomplished with `grep -c`.

``` bash
find /home/admin/ -maxdepth 1 -iname "*.txt" | xargs grep -c 'Alice'
```
![[Pasted image 20240817132404.png]]
where `xargs` passes the output of the .txt file search results to the `grep` command as arguments, where they scanned for the word **Alice** and then the flag `-c` makes the command count how many times it found the word.

Now, we filter only the word count of each file with `cut`

```bash
find /home/admin/ -maxdepth 1 -iname "*.txt" | xargs grep -c 'Alice' | cut --delimiter ":" --fields 2
```
![[Pasted image 20240817181048.png]]
where we use _:_ as a `--delimiter` to separate each line in two and then we choose the second `--fields`.

To sum this numbers, we'll first `paste` all of them in a single line with a _plus sign_ in between them.

```bash
find /home/admin/ -maxdepth 1 -iname "*.txt" | xargs grep -c 'Alice' | cut --delimiter ":" --fields 2 | paste -sd+ -
```
![[Pasted image 20240817181348.png]]

Finally, we give that output to the `bc` command to interpret and sum.

```bash
find /home/admin/ -maxdepth 1 -iname "*.txt" | xargs grep -c 'Alice' | cut --delimiter ":" --fields 2 | paste -sd+ - | bc
```
![[Pasted image 20240817181603.png]]

There are ___414___ mentions of **Alice** in total.
The  .text file that contains exactly one mention of **Alice** is ___1342-0.txt___.

With this information, we conclude that the answers is ___4141342___.





