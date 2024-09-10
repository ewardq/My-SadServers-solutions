---
dg-publish: true
---

# "Saint John": what is writing to this log file?
**Level:** Easy
**Type:** Fix
**Tags:**
**Description:** A developer created a testing program that is continuously writing to a log file _/var/log/bad.log_ and filling up disk. You can check for example with `tail -f /var/log/bad.log`.  
This program is no longer needed. Find it and terminate it.

**Test:** The log file size doesn't change (within a time interval bigger than the rate of change of the log file).

---
### Notes and solution:
You can use `top` to see all the running processes

![[Pasted image 20240815193543.png]]

or `ps aux` where:
- `ps` is the process status command
- `a` displays information about other users' processes as well as your own.
- `u` displays the processes belonging to the specified usernames.
- `x` includes processes that do not have a controlling terminal

![[Pasted image 20240815193619.png]]

Now, it seems that there is a process that runs every other time while using somewhat high resources. To filter this process we use `grep`.

```bash
top | grep badlog
```
![[Pasted image 20240815193827.png]]

or with `ps`

```bash
find -name "badlog.py"
```
![[Pasted image 20240815193935.png]]

Now we have identified the script that creates the bad log. We have to eliminate it and then stop the process.

Using `top` we can find the location of the script with the following command:

```bash
find -name "badlog.py"
```
![[Pasted image 20240815194259.png]]

As we can see, the script is in _/home/$USER/badlog.py_, so we delete that script and then kill the process related.

```bash
sudo find -name "badlog.py" | xargs rm` or `sudo rm /home/admin/badlog.py
```

```bash
kill 590
```

___Now the log file is no longer increasing in size___
