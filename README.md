# Linux Text Processing & VI Lab

**Text Processing + VI (Search / Replace Like a Pro)**

In this lab, I practiced **non-interactive text editing using `sed`** and **editor-based repetition and search/replace using `vi`**, while also reinforcing **proper user switching, sudo configuration, and safe destructive operations**.

This lab focuses on **editing text cleanly, repeatably, and correctly**, without relying on manual edits.

---

## 📋 Lab Overview

**Goal:**

* Switch users correctly using a full login shell
* Perform file creation in protected directories
* Safely remove directories recursively
* Use `sed` for non-interactive, in-place text replacement
* Use `vi` for bulk editing, copying, and global search/replace
* Validate results using counting and verification commands

**Learning Outcomes:**

* Understand the difference between `su user` and `su - user`
* Grant sudo access correctly using group membership
* Use `rm -rf` safely and intentionally
* Perform **case-sensitive** find & replace using `sed`
* Use `vi` command mode for repetition and global substitution
* Verify file changes using `cat` and `wc -l`

---

## 🛠 Step-by-Step Journey

### Step 1: Switch to `user2` with a Clean Environment

**Command:**

```bash
su - user2
```

* `-` starts a **login shell**
* Loads user2’s environment (`HOME`, `PATH`, shell configs)
* Places you directly in `/home/user2`

📌 **Why this matters:**
Using `su user2` without `-` keeps the original environment, which can cause subtle issues (wrong PATH, missing tools, wrong working directory).

**Verification:**

```bash
whoami
```

---

### Step 2: Create `/dir1/f2` (Fixing Sudo Access)

**Initial attempt:**

```bash
touch /dir1/f2
```

* ❌ Permission denied (protected directory)

**Retry with sudo:**

```bash
sudo touch /dir1/f2
```

* ❌ `user2` not in sudoers

**Fix: Add user2 to wheel group (as root):**

```bash
usermod -aG wheel user2
```

**Re-login to apply group membership:**

```bash
su - user2
```

**Create the file successfully:**

```bash
sudo touch /dir1/f2
```

**Verify:**

```bash
ls -l /dir1/f2
```

---

### Step 3: Delete Directories Recursively

**Task:** Remove `/dir6` and `/dir8`

**Command:**

```bash
sudo rm -rf /dir6 /dir8
```

* `-r` → recursive (directories + contents)
* `-f` → force (no prompts)

**Verification:**

```bash
ls -l /dir6 /dir8
```

* Both return *No such file or directory*

---

### Step 4: Non-Interactive Text Replacement with `sed`

**Task:** Replace `DevOps` → `devops` in `/f3` **without opening an editor**

**Inspect file first:**

```bash
cat /f3
```

**Command:**

```bash
sudo sed -i 's/DevOps/devops/g' /f3
```

**Breakdown:**

* `sed` → stream editor
* `-i` → edit file **in place**
* `s` → substitute
* `DevOps` → pattern to find (case-sensitive)
* `devops` → replacement
* `g` → global (all matches per line)

📌 Without `-i`, the change would only print to stdout.

**Verify:**

```bash
cat /f3
```

---

### Step 5: Copy a Line 10 Times Using `vi`

**Open file:**

```bash
sudo vi /f3
```

**Inside `vi`:**

* `ESC` → ensure command mode
* `yy` → yank (copy) current line
* `10p` → paste the line 10 times
* `:wq` → save and quit

---

### Step 6: Count Lines Using `wc`

**Command:**

```bash
wc -l /f3
```

**Explanation:**

* `wc` → word count utility
* `-l` → count **lines**
* Output confirms **11 lines**

---

### Step 7: Global Search & Replace Using `vi`

**Task:** Replace `Engineer` → `engineer` across entire file

**Command:**

```bash
sudo vi /f3
```

**Inside `vi`:**

```vim
:%s/Engineer/engineer/g
```

**Breakdown:**

* `:` → command-line mode
* `%` → entire file (all lines)
* `s` → substitute
* `Engineer` → search pattern
* `engineer` → replacement
* `g` → replace all matches per line

📌 Equivalent to `1,$s/Engineer/engineer/g`

**Save & exit:**

```vim
:wq
```

---

### Step 8: Delete the File

**Command:**

```bash
sudo rm -f /f3
```

**Verify:**

```bash
ls /f3
```

* No such file or directory

---

## ✅ Key Commands Summary

| Task                      | Command                               |
| ------------------------- | ------------------------------------- |
| Login shell switch        | `su - user2`                          |
| Verify current user       | `whoami`                              |
| Create protected file     | `sudo touch /dir1/f2`                 |
| Grant sudo via wheel      | `usermod -aG wheel user2`             |
| Recursive delete          | `sudo rm -rf /dir6 /dir8`             |
| In-place text replacement | `sudo sed -i 's/DevOps/devops/g' /f3` |
| Copy line in vi           | `yy`                                  |
| Paste line 10 times       | `10p`                                 |
| Count lines               | `wc -l /f3`                           |
| Global replace in vi      | `:%s/Engineer/engineer/g`             |
| Force delete file         | `sudo rm -f /f3`                      |

---

## 💡 Notes / Tips

* Always prefer `su - user` over `su user` for a **clean environment**
* Group changes require **re-login** to take effect
* `sed` is ideal for automation and scripts
* `vi` excels at interactive repetition and bulk edits
* `rm -rf` is powerful — verify paths before running
* `wc -l` counts lines, not metadata

---

## 📌 Lab Summary

| Step                  | Status | Key Takeaway                             |
| --------------------- | ------ | ---------------------------------------- |
| Login shell switching | ✅      | Clean environment avoids subtle bugs     |
| Sudo configuration    | ✅      | Wheel group enables controlled privilege |
| Recursive deletion    | ✅      | Multiple paths can be removed safely     |
| sed replacement       | ✅      | Non-interactive, in-place edits          |
| vi repetition         | ✅      | Fast duplication with yank & paste       |
| Line counting         | ✅      | `wc -l` validates content size           |
| Global replace        | ✅      | `%` applies changes to entire file       |
| Cleanup               | ✅      | Environment left clean                   |
