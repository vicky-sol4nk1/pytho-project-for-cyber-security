

# **Find Command Notes**

## **1. Basic Structure**

```bash
find [path] [conditions] [actions]
```

* `[path]` → directory to start search (e.g., `/`, `/home`, `/etc`)
* `[conditions]` → filters like name, type, permissions, size, etc.
* `[actions]` → what to do with found files (optional, e.g., `-print`, `-delete`)

---

## **2. Common Options**

### **a) File Type**

* `-type f` → regular files
* `-type d` → directories

### **b) Name Filtering**

* `-name "*.ext"` → case-sensitive filename match
* `-iname "*.ext"` → case-insensitive filename match

---

### **c) Permissions**

* `-perm -4000` → SUID files
* `-perm -2000` → SGID files
* `-perm -004` → readable by others
* `! -perm -004` → **not readable by others**

**Tip:** Leading zeros may or may not be required depending on system, e.g., `004` or `-0004`.

---

### **d) Ownership**

* `-user <username>` → files owned by a specific user
* `-group <groupname>` → files owned by a specific group

---

### **e) Time**

* `-mtime n` → modified exactly n days ago
* `-mtime +n` → modified more than n days ago
* `-mtime -n` → modified less than n days ago

---

### **f) Size**

* `-size +100M` → larger than 100 MB
* `-size -100M` → smaller than 100 MB
* Units:

  * `c` → bytes
  * `k` → KB
  * `M` → MB
  * `G` → GB

---

### **g) Empty Directories**

* `-empty` → empty files or directories
* Use with `-type d` to find empty directories

---

### **h) Writable / Executable**

* `-writable` → files you can write
* `-executable` → files you can execute

---

### **i) Suppressing Errors**

* `2>/dev/null` → hide “Permission denied” errors

---

## **3. Combining Filters**

* Use multiple conditions together:

```bash
find /path -type f -name "*.sh" -user root -perm -4000 -mtime -7 2>/dev/null
```

* Order doesn’t usually matter, but readability helps.
* Use `!` for negation:

```bash
! -perm -004   # not readable by others
```

---

## **4. Examples You Practiced**

| Task                                               | Command                                                       |
| -------------------------------------------------- | ------------------------------------------------------------- |
| Find SUID Python files                             | `find / -type f -iname "*python*" -perm -4000 2>/dev/null`    |
| Find writable shell scripts                        | `find / -type f -name "*.sh" -writable 2>/dev/null`           |
| Find files modified in last 1 day                  | `find / -type f -mtime -1 2>/dev/null`                        |
| Find root-owned SUID files modified in last 7 days | `find / -type f -user root -perm -4000 -mtime -7 2>/dev/null` |
| Find empty directories under /home                 | `find /home -type d -empty 2>/dev/null`                       |
| Find files larger than 100MB in /var/log           | `find /var/log -type f -size +100M 2>/dev/null`               |
| Find `.conf` files modified in last 3 days         | `find /etc -type f -name "*.conf" -mtime -3 2>/dev/null`      |
| Find files owned by `bunny` not readable by others | `find /home -type f -user bunny ! -perm -004 2>/dev/null`     |

---

✅ **Key Takeaways**

1. `find` is very powerful for searching **by name, size, permissions, owner, time, and type**.
2. Always use `2>/dev/null` if you don’t want permission errors flooding your terminal.
3. Combine multiple filters for precise searches.
4. Remember the difference between `+` (greater than), `-` (less than), and exact values for `mtime` and `size`.

=
