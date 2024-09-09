**Scenario:** "Apia": Needle in a Haystack

**Level:** Easy

**Type:** Do

**Tags:** [pro](https://sadservers.com/tag/pro)  

**Description:** In a directory _/home/admin/data_, there are multiple files, all of them with same content. One of these files has been modified, a word was added. You need to identify which word it is and put it in the solution file (both newline terminated or not are accepted).

**Test:** `md5sum /home/admin/solution` should return `55aba155290288b58e9b778c8f616560` or `2eeefea9fc4b16ea624bed5c67a49d80`  
  
Check My Solution: The "Check My Solution" button runs the script _/home/admin/agent/check.sh_, which you can

---
_Notes and solution_:  First, we see how many files are in the target directory.

```bash
cd /home/admin/data
ls
```
![[Pasted image 20240909142658.png]]

We'll use the file #0 as a basis to compare the rest of the files. If _file1.txt_ is the odd one out, the `diff` command will show that all of the files are different; if not, it'll only show the file that's different from the others.

We'll list all the files with the `ls` command and pass them as arguments (using `xargs`) to the `diff` command

`ls | xargs diff -c --color --from-file file0.txt `
![[Pasted image 20240909142751.png]]
where 
- `--color` highlights the changes between files (green meaning a line was added and red, deleted).
- `-c` adds context to the result of the command, like where it was found and in which file.
- `--from-file` makes it possible to compare _file0.txt_ with multiple files.

And now we see that the file that's different from the rest is **file76.txt**.
Additionally, each line consists of a lot of text so it is difficult to find the difference between each one.

At this point, it's only a matter of looking word by word which to find the odd one out. Who has time for that?

The proposed solution is to separate each word in a line, save that list in a new file and then compare it to the second converted file using `diff`.

1. **Separate text into a list of words**: To achieve this, we'll use the `awk` command with the `gsub` utility to substitute each space with a new line. 

`cat file0.txt | awk '{ gsub(" ", "\n") } 1'`
![[Pasted image 20240909143115.png]]
where the `1` at the end makes the command print $0 at the end of execution.

2. **Save the result in a temporary file**, to `diff` them later.

```bash
cat file0.txt | awk '{ gsub(" ", "\n") } 1' > tempfile1.txt
cat file76.txt | awk '{ gsub(" ", "\n") } 1' > tempfile2.txt
```

3. **Compare the two new files together**

`diff --color tempfile1.txt tempfile2.txt`
![[Pasted image 20240909143445.png]]

And now we see that the secret word is ___eureka___.

![[Pasted image 20240909143844.png]]
