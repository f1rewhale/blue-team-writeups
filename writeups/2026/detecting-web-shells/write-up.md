# Detecting Web Shells

**Platform:** TryHackMe  
**Difficulty:** Easy  
**Date:** 2026-09-19

## Overview

Room about detecting web shells from a defensive perspective. Looking at logs, the file system, and attacker behavior after the shell is uploaded.

## What happened

The attacker found an upload form, uploaded a PHP web shell (`shadyshell.php`), and started executing commands through it.  
First command was `whoami`. Later they downloaded `linpeas.sh`.

Key details:
- Attacker IP: `203.0.113.66`
- Upload path related to WordPress
- Web shell name: `shadyshell.php`

## Blue Team Takeaways

**Detection ideas:**
- New `.php` files appearing in upload directories
- Requests with parameters like `?cmd=` or similar
- Web server user (`www-data`) spawning unusual processes

**Hunting:**
- Search access logs for suspicious parameters
- Look for rare PHP files in `/wp-content/uploads/`
- Check for known tools being downloaded after a suspicious web request

**Mitigations:**
- Don’t allow PHP execution in upload folders
- Proper file type validation + renaming uploaded files
- File integrity monitoring on web directories

## Lessons Learned

Web shells are often easy to spot if you have decent logging and monitor file creation in web directories.  
Even basic detections around uploads and suspicious parameters can catch a lot of cases.

---

*Focused on defensive value.*
