
## 1️⃣ What is `find` (simple words)

`find` = **search files/directories** based on conditions:

* name
* permission
* owner
* size
* time
* SUID/SGID bits
* writable files (very important for privesc)

Syntax (base):

```bash
find <where_to_search> <condition> <action>
```

Example:

```bash
find / -name passwd
```

---

## 2️⃣ Red Team Rule #1 (VERY IMPORTANT)

When running `find /`, you’ll get **permission denied spam**.
Always suppress it:

```bash
2>/dev/null
```

Example:

```bash
find / -name passwd 2>/dev/null
```

🧠 `2>` = stderr
`/dev/null` = black hole 😄

---

## 3️⃣ Find by NAME (basic)

```bash
find / -name shadow 2>/dev/null
```

Case-insensitive:

```bash
find / -iname "*pass*" 2>/dev/null
```

Red team use:

* config files
* passwords
* backups

```bash
find / -iname "*backup*" 2>/dev/null
```

---

## 4️⃣ Find FILE TYPES (important)

### Regular files

```bash
find / -type f 2>/dev/null
```

### Directories

```bash
find / -type d 2>/dev/null
```

### Symbolic links

```bash
find / -type l 2>/dev/null
```

---

## 5️⃣ Find by PERMISSIONS (🔥 privesc gold)

### World-writable files

```bash
find / -type f -perm -0002 2>/dev/null
```

### World-writable directories

```bash
find / -type d -perm -0002 2>/dev/null
```

🧠 Why important?

* Writable scripts run by root = **root shell**

---

## 6️⃣ Find SUID files (🔥🔥 MOST IMPORTANT)

SUID binaries run as **owner (often root)**.

```bash
find / -perm -4000 -type f 2>/dev/null
```

You should instantly check:

* `/usr/bin`
* `/bin`
* `/sbin`

Look for **weird binaries**, not common ones.

🧠 Red team thinking:

> “Can I abuse this binary to spawn a shell?”

---

## 7️⃣ Find SGID files

```bash
find / -perm -2000 -type f 2>/dev/null
```

Less common, but still useful.

---

## 8️⃣ Find files owned by ROOT but writable by you

```bash
find / -user root -writable 2>/dev/null
```

🔥 If you can modify a root-owned file → **game over**.

---

## 9️⃣ Find recently modified files (loot hunting)

Last 1 day:

```bash
find / -mtime -1 2>/dev/null
```

Last 7 days:

```bash
find / -mtime -7 2>/dev/null
```

Red team use:

* recently edited scripts
* cron jobs
* temp files

---

## 🔟 Find CRON related files (priv esc classic)

```bash
find /etc -iname "*cron*" 2>/dev/null
```

Also:

```bash
find / -iname "*.sh" 2>/dev/null
```

Writable shell scripts = 🚩

---

## 1️⃣1️⃣ Find files with EXEC permission

```bash
find / -type f -executable 2>/dev/null
```

Look for:

* custom scripts
* unknown binaries

---

## 1️⃣2️⃣ Combine conditions (advanced but powerful)

Example: **root-owned + executable + writable**

```bash
find / -user root -type f -executable -writable 2>/dev/null
```

This is **pure red team gold**.

---

## 1️⃣3️⃣ `-exec` (careful but powerful)

Run a command on found files.

Example:

```bash
find / -name "*.conf" -exec ls -l {} \; 2>/dev/null
```

🧠 `{}` = found file
`\;` = end of exec

⚠️ Red team tip: **observe first, don’t modify blindly**.

---

## 1️⃣4️⃣ Red Team Cheat Mindset 🧠

When you run `find`, always ask:

* ❓ Is it writable?
* ❓ Is it executed by root?
* ❓ Is it SUID?
* ❓ Can I inject a command?

---

## 1️⃣5️⃣ Most Important `find` Commands (MEMORIZE)

```bash
find / -perm -4000 -type f 2>/dev/null
find / -type f -perm -0002 2>/dev/null
find / -user root -writable 2>/dev/null
find / -mtime -1 2>/dev/null
find / -iname "*pass*" 2>/dev/null
```


