---
dg-publish: true
---
---
**Type:** Fix
**Description:** Can't ping google.com. It returns ping: google.com: Name or service not known. Expected is being able to resolve the hostname. (Note: currently the VMs can't ping outside so there's no automated check for the solution).

**Test:** ping google.com should return something like PING google.com (172.217.2.46) 56(84) bytes of data.

---
### Notes and solution:
