---
dg-publish: true
---
---
#bash #find #grep #DataStreams
**Type:** Do
**Description:** A spy has left a password in a file in _/proc/sys_ . The contents of the file start with _"secret:"_ (without the quotes).  
  
Find the file and save the word after "secret:" to the file _/home/admin/secret.txt_ with a newline at the end (e.g. if the file contents were "secret:password" do: `echo "password" > /home/admin/secret.txt`).  
  
(Note there's no root/sudo access in this scenario).

**Test:** Running `md5sum /home/admin/secret.txt` returns `a7fcfd21da428dd7d4c5bb4c2e2207c4`  
  
The "Check My Solution" button runs the script _/home/admin/agent/check.sh_, which you can see and execute.

---
### Notes and solution:
To find a word in the contents of all of the files in _/proc/sys_ use the `find` command:
``` bash
find /proc/sys -type f | xargs grep 'secret:'
```
![[Pasted image 20240929153549.png]]

There are a lot of files that can't be accessed without root/sudo access, which we don't have in this scenario. To filter this error messages, redirect **only the output of the command, not the errors**.

``` bash
find /proc/sys -type f | xargs grep 'secret:' 1> output.txt
cat output.txt
```

![[Pasted image 20240929154743.png]]

 The secret password is ___excalibur___