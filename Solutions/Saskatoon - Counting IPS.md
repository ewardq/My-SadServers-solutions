---
dg-publish: true
---

# "Saskatoon": counting IPs.
**Level:** Easy
**Description:** There's a web server access log file at 
_/home/admin/access.log_. The file consists of one line per HTTP request, with the requester's IP address at the beginning of each line.  
  
Find what's the IP address that has the most requests in this file (there's no tie; the IP is unique). Write the solution into a file _/home/admin/highestip.txt_. For example, if your solution is "1.2.3.4", you can do `echo "1.2.3.4" > /home/admin/highestip.txt`

**Test:** The SHA1 checksum of the IP address `sha1sum /home/admin/highestip.txt` is `6ef426c40652babc0d081d438b9f353709008e93` (just a way to verify the solution without giving it away.)

---
### Notes and solution:
The first step is to open the log file to see what we are working with.

```bash
less /home/admin/access.log
```
![[Pasted image 20240815212040.png]]

There is a lot of information in that file.  If we count the number of lines in that file using:

```bash
wc -l /home/admin/access.log
```
![[Pasted image 20240815212223.png]]

First, we filter all the IPs with `grep` ( `-E`nhanced and exact match `o` options) and a RegEx pattern.

```bash
grep -E -o "([0-9]{1,3}[\.]){3}[0-9]{1,3}" /home/admin/access.log
```
![[Pasted image 20240815212819.png]]
where the RegEx pattern matches three digits `[0-9]{1,3}` between 0 and 9, followed by a dot `\.`, three times `{3}` and then a final set of three digits also between 0 and 9 `[0-9]{1,3}`.

then, we sort (so we can later use `uniq`) and filter with the utility to count each time a unique IP address appears.

```bash 
grep -E -o "([0-9]{1,3}[\.]){3}[0-9]{1,3}" /home/admin/access.log | sort | uniq
```
![[Pasted image 20240815214156.png]]
for instance, we can see that the _1.22.35.226_ IP address appears 6 times.

now, we want to know which is the most recurrent IP address, so we sort again numerically with the command `sort -n`umerically.

```bash 
grep -E -o "([0-9]{1,3}[\.]){3}[0-9]{1,3}" /home/admin/access.log | sort | uniq -c | sort -n`
```
![[Pasted image 20240815214525.png ]]

As we can see, the most recurring IP address is ___66.249.73.135___ with 482 requests.

Special thanks to [this ]([Extracting IP address from logfile (youtube.com)](https://www.youtube.com/watch?v=WDjbMucvEmk)) tutorial.

