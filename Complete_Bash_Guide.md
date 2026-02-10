# Complete Bash Guide 🚀

> **Your all-in-one reference for Bash - from basics to advanced**  
> Combines 200+ essential commands + 70+ advanced topics

---

## 📖 How to Use This Guide

- **Beginners**: Start with Part 1 (Essential Commands)
- **Intermediate**: Jump to specific topics you need
- **Advanced**: Explore Part 2 for power-user features
- **Quick Reference**: See the command summary at the end

**Symbols:**
- 💡 = Pro tips and best practices
- ⚠️ = Warnings and common pitfalls
- 🔥 = Advanced/powerful features

---

## 📚 Table of Contents

### Part 1: Essential Commands
1. [Command History](#1-command-history)
2. [Directory Operations](#2-directory-operations)
3. [File Operations](#3-file-operations)
4. [File Permissions](#4-file-permissions)
5. [Finding Files](#5-finding-files)
6. [Searching & Replacing (grep/sed)](#6-searching--replacing)
7. [Symbolic Links](#7-symbolic-links)
8. [Compression & Archives](#8-compression--archives)
9. [System Resources](#9-system-resources)
10. [Process Management](#10-process-management)
11. [Network Operations](#11-network-operations)
12. [Scheduled Tasks (cron/at)](#12-scheduled-tasks)
13. [Remote Access (SSH/SCP)](#13-remote-access)
14. [Bash Scripting Basics](#14-bash-scripting-basics)

### Part 2: Advanced Topics
15. [Text Processing (awk/cut/tr/sort/uniq)](#15-text-processing)
16. [Stream Editors (tee/xargs)](#16-stream-editors--filters)
17. [Advanced Scripting (arrays/strings/traps)](#17-advanced-scripting-features)
18. [User & Permission Management](#18-user--permission-management)
19. [Disk & Filesystem Management](#19-disk--filesystem-management)
20. [System Logging & Monitoring](#20-system-logging--monitoring)
21. [File Utilities (diff/file/base64)](#21-file-utilities)
22. [Hashing & Checksums](#22-hashing--checksums)
23. [Shell Customization](#23-shell-customization)

---

# Part 1: Essential Commands

## 1. Command History

```shell
!!                          # Run the last command
!$                          # Last argument of last command
!*                          # All arguments of last command
!123                        # Run command #123 from history
!ssh                        # Run last command starting with 'ssh'
^old^new                    # Replace 'old' with 'new' in last command

history                     # Show command history
history 20                  # Show last 20 commands
Ctrl+R                      # Search history interactively
```

**💡 Example:**
```shell
touch important_file.txt
chmod +x !$                 # Adds execute permission to important_file.txt
```

---

## 2. Directory Operations

### Navigation
```shell
pwd                         # Print current directory
ls                          # List files
ls -la                      # List all with details
ls -lh                      # Human-readable sizes
ls -lt                      # Sort by modification time
cd /path/to/dir             # Change directory
cd ~                        # Go to home directory
cd -                        # Go to previous directory
pushd /path                 # Go to path and save current location
popd                        # Return to saved location
tree                        # Show directory tree
```

### Creating & Modifying
```shell
mkdir folder                # Create directory
mkdir -p path/to/folder     # Create nested directories
mkdir {a,b,c}               # Create multiple directories
mktemp -d                   # Create temporary directory

cp -r source/ dest/         # Copy directory
mv old/ new/                # Move/rename directory
rsync -avz src/ dest/       # Sync directories (better than cp)

rmdir folder                # Delete empty directory
rm -r folder                # Delete directory and contents
rm -rf folder               # Force delete (no confirmation)
```

**💡 rsync advantages:** Shows progress, resumes interrupted transfers, preserves permissions

---

## 3. File Operations

### Creating Files
```shell
touch file.txt              # Create empty file
touch {a,b,c}.txt           # Create multiple files
touch file{1..10}.txt       # Create file1.txt through file10.txt
echo "text" > file.txt      # Create file with content (overwrite)
echo "text" >> file.txt     # Append to file
```

### Reading Files
```shell
cat file.txt                # Print entire file
less file.txt               # View with pagination (q to quit)
head file.txt               # First 10 lines
head -n 20 file.txt         # First 20 lines
tail file.txt               # Last 10 lines
tail -f file.txt            # Follow file in real-time (logs)
wc file.txt                 # Count lines, words, characters
wc -l file.txt              # Count lines only
```

### Copying & Moving
```shell
cp source.txt dest.txt      # Copy file
cp -i source.txt dest.txt   # Interactive (confirm overwrite)
mv old.txt new.txt          # Move/rename file
rsync -avz file.txt backup/ # Efficient file copy
```

### Deleting
```shell
rm file.txt                 # Delete file
rm -f file.txt              # Force delete
rm -i *.txt                 # Interactive delete
```

### Redirection
```shell
command > output.txt        # Redirect stdout (overwrite)
command >> output.txt       # Append stdout
command 2> errors.txt       # Redirect stderr
command &> all.txt          # Redirect both stdout and stderr
command > /dev/null 2>&1    # Discard all output
```

---

## 4. File Permissions

### Permission System
```
Number  Binary  Meaning
7       111     read + write + execute (rwx)
6       110     read + write (rw-)
5       101     read + execute (r-x)
4       100     read only (r--)
3       011     write + execute (-wx)
2       010     write only (-w-)
1       001     execute only (--x)
0       000     no permissions (---)
```

### Common Patterns
```
755     rwxr-xr-x    Executable files/directories
644     rw-r--r--    Regular files
600     rw-------    Private files (only owner)
777     rwxrwxrwx    Everything (⚠️ avoid in production)
```

### Changing Permissions
```shell
chmod 755 script.sh         # Numeric mode
chmod u+x script.sh         # Add execute for user
chmod g+w file.txt          # Add write for group
chmod o-r file.txt          # Remove read for others
chmod a+x script.sh         # Add execute for all
chown user:group file.txt   # Change owner and group
chown -R user:group dir/    # Recursive change
```

**Permission Groups:** u=user, g=group, o=others, a=all

---

## 5. Finding Files

### Locate Commands
```shell
which python                # Find command location
type ls                     # Show command type
whereis nginx               # Find binary, source, manual
```

### Using find (Real-time Search)
```shell
find . -name "file.txt"     # Find by name
find . -iname "*.PDF"       # Case-insensitive
find . -type f              # Find files only
find . -type d              # Find directories only
find . -size +100M          # Files larger than 100MB
find . -mtime -7            # Modified in last 7 days
find . -name "*.tmp" -delete # Find and delete
find . -name "*.log" -exec chmod 644 {} \; # Execute command on each
```

### Using locate (Fast, Indexed Search)
```shell
sudo updatedb               # Update file index
locate filename             # Find file
locate -i filename          # Case-insensitive
locate "*.pdf"              # Pattern search
```

**💡 When to use:** `find` for accuracy, `locate` for speed

---

## 6. Searching & Replacing

### grep - Search in Files
```shell
grep "pattern" file.txt     # Search for pattern
grep -r "pattern" dir/      # Recursive search
grep -i "pattern" file      # Case-insensitive
grep -v "pattern" file      # Invert match (exclude)
grep -n "pattern" file      # Show line numbers
grep -c "pattern" file      # Count matches
grep -l "pattern" *         # Show only filenames
grep -A 3 "pattern" file    # Show 3 lines after match
grep -B 3 "pattern" file    # Show 3 lines before match
grep -C 3 "pattern" file    # Show 3 lines around match
grep -E "pat1|pat2" file    # Multiple patterns (regex)
```

**💡 Real example:** Find all TODOs in code:
```shell
grep -rn "TODO\|FIXME" --include="*.js" .
```

### sed - Find and Replace
```shell
sed 's/old/new/' file       # Replace first occurrence per line
sed 's/old/new/g' file      # Replace all occurrences
sed 's/old/new/gi' file     # Case-insensitive replace
sed 's/old/new/g' file > new.txt # Save to new file
sed -i 's/old/new/g' file   # Edit file in-place
sed -i.bak 's/old/new/g' file # Keep backup
```

---

## 7. Symbolic Links

```shell
ln -s /path/to/target link  # Create symbolic link
ln -sf /path/to/target link # Force overwrite
ls -l                       # View links and targets
readlink link               # Show link target
```

**💡 Example:** Link dotfiles from version control:
```shell
ln -s ~/dotfiles/.bashrc ~/.bashrc
```

---

## 8. Compression & Archives

### Compress
```shell
# zip (cross-platform)
zip archive.zip file.txt
zip -r archive.zip directory/

# gzip (single file)
gzip file.txt               # Creates file.txt.gz
gzip -k file.txt            # Keep original

# tar (multiple files/dirs)
tar -czf archive.tar.gz file1 file2 dir/
tar -czf backup.tar.gz --exclude="*.log" /data/
```

### Decompress
```shell
unzip archive.zip
unzip archive.zip -d /dest/

gunzip file.gz
gunzip -k file.gz           # Keep .gz file

tar -xzf archive.tar.gz
tar -xzf archive.tar.gz -C /dest/
```

**💡 tar flags:** c=create, x=extract, z=gzip, f=file

---

## 9. System Resources

### Disk Usage
```shell
df -h                       # Disk space (all drives)
du -sh *                    # Size of each item in current dir
du -h --max-depth=1         # One level deep
du -sh /path/to/dir         # Total size of directory
```

**💡 Find largest directories:**
```shell
du -h / | sort -rh | head -20
```

### Memory
```shell
free -h                     # RAM usage
free -h --si                # Use 1000 instead of 1024
top                         # Interactive process monitor
htop                        # Better process monitor
```

### Package Management (Debian/Ubuntu)
```shell
sudo apt update             # Update package list
apt search nginx            # Search package
sudo apt install nginx      # Install package
sudo apt remove nginx       # Remove package
sudo apt upgrade            # Upgrade all packages
```

### Shutdown & Reboot
```shell
sudo shutdown now           # Shutdown immediately
sudo shutdown +5            # Shutdown in 5 minutes
sudo shutdown -r now        # Reboot now
sudo reboot                 # Reboot
```

---

## 10. Process Management

### View Processes
```shell
ps aux                      # All processes
ps aux | grep nginx         # Find specific process
top                         # Interactive monitor
htop                        # Better monitor
pidof nginx                 # Get PID by name
pgrep nginx                 # Same as pidof
lsof -i :8080               # What's using port 8080?
```

### Control Processes
```shell
command &                   # Run in background
Ctrl+Z                      # Suspend foreground process
bg                          # Resume in background
fg                          # Bring to foreground
jobs                        # List background jobs
jobs -p                     # List with PIDs
```

### Process Priority
```shell
nice -n 10 command          # Start with lower priority
sudo renice -5 PID          # Change priority (higher)
ps -o pid,ni,comm           # View priorities
```

### Kill Processes
```shell
Ctrl+C                      # Kill foreground process
kill PID                    # Graceful shutdown (SIGTERM)
kill -9 PID                 # Force kill (SIGKILL)
pkill process_name          # Kill by name
killall process_name        # Kill all with name
```

---

## 11. Network Operations

### HTTP Requests
```shell
curl https://api.example.com           # GET request
curl -i https://api.example.com        # Include headers
curl -L https://example.com            # Follow redirects
curl -o file.pdf https://url/file.pdf  # Download
curl -H "Auth: token" https://api.com  # Custom header
curl -X POST -d "data" https://api.com # POST request
wget https://file.zip                  # Download with wget
```

### Network Troubleshooting
```shell
ping google.com             # Test connectivity
ping -c 5 google.com        # Send 5 pings
ip addr                     # Network interfaces
ip route                    # Routing table
netstat -tuln               # Listening ports
traceroute google.com       # Route to destination
nmap localhost              # Port scan
```

### DNS
```shell
host google.com             # DNS lookup
dig google.com              # Detailed DNS info
nslookup google.com         # Query DNS
cat /etc/resolv.conf        # DNS servers
```

---

## 12. Scheduled Tasks

### Cron (Recurring)
```
Format: * * * * *
        │ │ │ │ └─ Day of week (0-7)
        │ │ │ └─── Month (1-12)
        │ │ └───── Day of month (1-31)
        │ └─────── Hour (0-23)
        └───────── Minute (0-59)
```

```shell
crontab -l                  # List cron jobs
crontab -e                  # Edit cron jobs
```

**Common Examples:**
```shell
* * * * * script.sh         # Every minute
*/15 * * * * script.sh      # Every 15 minutes
0 2 * * * backup.sh         # Daily at 2 AM
0 0 * * 0 weekly.sh         # Weekly on Sunday
@reboot startup.sh          # At system boot
```

### at (One-time)
```shell
at now + 2 minutes          # Schedule task
at 10:30 PM                 # Specific time
at tomorrow                 # Tomorrow
at -l                       # List scheduled tasks
at -r 1                     # Remove task #1
```

---

## 13. Remote Access

### SSH
```shell
ssh user@hostname           # Connect to server
ssh -p 2222 user@host       # Custom port
ssh -i key.pem user@host    # Use key file
```

**SSH Config (~/.ssh/config):**
```
Host myserver
    HostName 192.168.1.100
    User admin
    Port 2222
```
Then: `ssh myserver`

### SCP (Secure Copy)
```shell
scp file.txt user@host:/path/       # Upload file
scp user@host:/path/file.txt .      # Download file
scp -r dir/ user@host:/path/        # Upload directory
scp -P 2222 file.txt user@host:/    # Custom port
```

**💡 Better alternative:** Use `rsync` over SSH:
```shell
rsync -avz -e ssh dir/ user@host:/path/
```

---

## 14. Bash Scripting Basics

### Script Template
```bash
#!/bin/bash
# Script description

set -euo pipefail           # Exit on error, undefined var, pipe failure

# Your code here
echo "Hello World"
```

Make executable: `chmod +x script.sh`

### Variables
```bash
name="John"                 # No spaces around =
age=25
echo "Hello $name"          # Use $variable
echo "Hello ${name}!"       # Safer with ${...}
readonly PI=3.14            # Read-only variable
```

### Functions
```bash
greet() {
    local name=$1           # Local variable
    echo "Hello, $name!"
    return 0
}

greet "Alice"
result=$(greet "Bob")       # Capture output
```

### Conditionals
```bash
# Numeric comparison
if [ $age -eq 18 ]; then echo "Adult"; fi
if [ $age -gt 18 ]; then echo "Over 18"; fi
if [ $age -lt 18 ]; then echo "Under 18"; fi

# String comparison
if [ "$name" = "John" ]; then echo "Match"; fi
if [ -z "$name" ]; then echo "Empty"; fi
if [ -n "$name" ]; then echo "Not empty"; fi

# File tests
if [ -f file.txt ]; then echo "File exists"; fi
if [ -d folder ]; then echo "Directory exists"; fi
if [ -x script.sh ]; then echo "Executable"; fi

# If-elif-else
if [ $age -lt 13 ]; then
    echo "Child"
elif [ $age -lt 20 ]; then
    echo "Teen"
else
    echo "Adult"
fi
```

### Loops
```bash
# While loop
counter=1
while [ $counter -le 5 ]; do
    echo $counter
    ((counter++))
done

# For loop (range)
for i in {1..5}; do
    echo $i
done

# For loop (files)
for file in *.txt; do
    echo "Processing $file"
done
```

### Case Statement
```bash
read -p "Enter fruit: " fruit
case $fruit in
    apple)
        echo "Red"
        ;;
    banana)
        echo "Yellow"
        ;;
    *)
        echo "Unknown"
        ;;
esac
```

---

# Part 2: Advanced Topics

## 15. Text Processing

### awk - Column Processing
```shell
awk '{print $1}' file       # Print 1st column
awk '{print $1, $3}' file   # Print 1st and 3rd columns
awk -F',' '{print $2}' data.csv # CSV: print 2nd column
awk '/error/ {print $0}' log    # Lines containing "error"
awk '$3 > 100' data         # Lines where 3rd column > 100
awk '{sum += $1} END {print sum}' # Sum 1st column
```

### cut - Extract Columns
```shell
cut -d',' -f1 data.csv      # 1st field from CSV
cut -d',' -f1,3,5 data.csv  # Fields 1, 3, 5
cut -c1-10 file.txt         # Characters 1-10
```

### tr - Transform Characters
```shell
echo "hello" | tr '[:lower:]' '[:upper:]'  # HELLO
echo "hello123" | tr -d '0-9'              # hello
cat file.txt | tr '\n' ','                 # Newlines to commas
```

### sort & uniq
```shell
sort file.txt               # Alphabetical sort
sort -n numbers.txt         # Numeric sort
sort -r file.txt            # Reverse sort
sort -k2 file.txt           # Sort by 2nd column
sort -u file.txt            # Sort + remove duplicates

sort file.txt | uniq        # Remove duplicates
sort file.txt | uniq -c     # Count occurrences
sort file.txt | uniq -d     # Show only duplicates
```

### paste & column
```shell
paste file1.txt file2.txt   # Merge files side-by-side
paste -d',' file1 file2     # Comma-separated
column -t file.txt          # Auto-align columns
mount | column -t           # Format output nicely
```

---

## 16. Stream Editors & Filters

### tee - Write to File and stdout
```shell
ls | tee output.txt         # Display AND save
ls | tee -a output.txt      # Append
command | tee log.txt | grep error # Save all, filter display
```

### xargs - Build Commands from Input
```shell
find . -name "*.tmp" | xargs rm     # Delete all .tmp files
cat urls.txt | xargs -n 1 curl -O   # Download each URL
echo "file1 file2" | xargs touch    # Create files
ls *.txt | xargs -I {} cp {} backup/ # Copy each file
```

### Advanced sed
```shell
sed -n '5p' file            # Print line 5 only
sed -n '10,20p' file        # Print lines 10-20
sed '5d' file               # Delete line 5
sed '/pattern/d' file       # Delete matching lines
sed '5i\Text' file          # Insert before line 5
sed '5a\Text' file          # Append after line 5
```

---

## 17. Advanced Scripting Features

### Arrays
```bash
fruits=("apple" "banana" "orange")
echo ${fruits[0]}           # apple
echo ${fruits[@]}           # All elements
echo ${#fruits[@]}          # Length
fruits+=("grape")           # Append

for fruit in "${fruits[@]}"; do
    echo $fruit
done
```

### Associative Arrays
```bash
declare -A person
person[name]="John"
person[age]=30
echo ${person[name]}        # John
echo ${!person[@]}          # All keys
```

### String Manipulation
```bash
text="Hello World"
echo ${#text}               # Length: 11
echo ${text:0:5}            # Substring: Hello
echo ${text/World/Bash}     # Replace: Hello Bash
echo ${text//o/O}           # Replace all: HellO WOrld

file="path/to/file.txt"
echo ${file##*/}            # file.txt (basename)
echo ${file%.*}             # path/to/file (remove extension)
```

### Here Documents
```bash
cat << EOF
Multi-line
text block
Variables like $HOME work here
EOF

cat << EOF > config.txt
server=localhost
port=8080
EOF
```

### Traps - Signal Handling
```bash
cleanup() {
    echo "Cleaning up..."
    rm -f /tmp/tempfile
}

trap cleanup EXIT           # Run on script exit
trap cleanup INT            # Run on Ctrl+C
trap 'echo "Error line $LINENO"' ERR # On error
```

### Debugging
```bash
set -x                      # Enable debug (print commands)
set -e                      # Exit on error
set -u                      # Exit on undefined variable
set -o pipefail             # Exit if any pipe command fails
bash -x script.sh           # Run with debug output
```

---

## 18. User & Permission Management

### User Management
```shell
sudo useradd -m john        # Create user with home dir
sudo passwd john            # Set password
sudo usermod -aG sudo john  # Add to sudo group
sudo userdel -r john        # Delete user and home dir
```

### Switch Users
```shell
su - john                   # Switch to user john
sudo command                # Run as root
sudo -u john command        # Run as specific user
```

### Ownership
```shell
sudo chown user file.txt    # Change owner
sudo chown user:group file  # Change owner and group
sudo chown -R user:group dir/ # Recursive
```

### View User Info
```shell
whoami                      # Current user
id                          # User ID and groups
groups                      # Current user's groups
w                           # Who's logged in
last                        # Login history
```

---

## 19. Disk & Filesystem Management

### View Disks
```shell
lsblk                       # List block devices
lsblk -f                    # Include filesystem info
sudo fdisk -l               # List partitions
df -h                       # Mounted filesystems
sudo blkid                  # UUIDs
```

### Mounting
```shell
sudo mount /dev/sdb1 /mnt   # Mount partition
sudo umount /mnt            # Unmount
mount | column -t           # View all mounts
```

### Create Filesystem
```shell
sudo mkfs.ext4 /dev/sdb1    # Create ext4 filesystem
sudo mkfs.vfat /dev/sdb1    # Create FAT32
```

### Check & Repair
```shell
sudo fsck /dev/sdb1         # Check filesystem (unmounted only!)
sudo fsck -y /dev/sdb1      # Auto-fix errors
```

### Disk Imaging
```shell
sudo dd if=/dev/sda of=backup.img bs=4M status=progress
sudo dd if=backup.img of=/dev/sdb
```

---

## 20. System Logging & Monitoring

### systemd Logs
```shell
sudo journalctl             # All logs
sudo journalctl -u nginx    # Service logs
sudo journalctl -f          # Follow logs
sudo journalctl --since "1 hour ago"
sudo journalctl -p err      # Errors only
```

### Kernel Messages
```shell
dmesg                       # Kernel messages
dmesg | tail                # Recent messages
dmesg | grep -i error       # Search errors
dmesg -w                    # Follow
```

### Monitoring
```shell
watch -n 2 df -h            # Monitor disk every 2 sec
watch -d free -h            # Highlight changes
tail -f /var/log/syslog     # Follow system log
uptime                      # System uptime
```

---

## 21. File Utilities

### Comparison
```shell
diff file1.txt file2.txt    # Show differences
diff -u file1 file2         # Unified format
diff -y file1 file2         # Side-by-side
diff -r dir1/ dir2/         # Compare directories
cmp file1 file2             # Binary comparison
```

### File Type
```shell
file document.pdf           # Determine file type
file -i document.txt        # MIME type
```

### Encoding
```shell
iconv -f UTF-8 -t ISO-8859-1 in.txt > out.txt
dos2unix file.txt           # Windows to Unix
unix2dos file.txt           # Unix to Windows
```

### Base64
```shell
echo "Hello" | base64       # Encode
echo "SGVsbG8K" | base64 -d # Decode
base64 file.txt > encoded.txt
```

---

## 22. Hashing & Checksums

### Generate
```shell
md5sum file.txt             # MD5 hash
sha256sum file.txt          # SHA-256 hash
sha512sum file.txt          # SHA-512 hash
sha256sum *.iso > checksums.txt
```

### Verify
```shell
sha256sum -c checksums.txt  # Verify all files
```

---

## 23. Shell Customization

### Aliases (~/.bashrc)
```bash
alias ll='ls -lah'
alias update='sudo apt update && sudo apt upgrade'
alias ..='cd ..'
alias grep='grep --color=auto'
```

### History Configuration
```bash
HISTSIZE=10000              # Commands in memory
HISTFILESIZE=20000          # Commands in file
HISTCONTROL=ignoredups      # No duplicates
shopt -s histappend         # Append, don't overwrite
```

### Useful Functions
```bash
# Auto-ls after cd
cd() {
    builtin cd "$@" && ls
}

# Create and enter directory
mkcd() {
    mkdir -p "$1" && cd "$1"
}

# Extract any archive
extract() {
    if [ -f "$1" ]; then
        case "$1" in
            *.tar.gz)  tar -xzf "$1"   ;;
            *.zip)     unzip "$1"      ;;
            *.rar)     unrar x "$1"    ;;
            *)         echo "Unknown format" ;;
        esac
    fi
}
```

### Process Control
```shell
nohup command &             # Run immune to hangups
disown                      # Detach from shell
timeout 10s command         # Kill after 10 seconds
```

---

## Quick Reference Card

### Most Common Commands
```shell
# Navigation
pwd, cd, ls, tree

# Files
cat, less, head, tail, touch, cp, mv, rm, ln

# Search
find, locate, grep, which

# Text
awk, sed, cut, sort, uniq, wc, tr

# System
ps, top, kill, df, du, free, uptime

# Network
curl, wget, ping, ssh, scp, netstat

# Archive
tar, zip, unzip, gzip, gunzip

# Permissions
chmod, chown, sudo

# Package
apt install/remove/update/search

# Process
&, bg, fg, jobs, nohup, Ctrl+Z, Ctrl+C
```

---

## Pro Tips for Productivity

1. **Tab Completion**: Press Tab to autocomplete
2. **Ctrl+R**: Search command history
3. **Ctrl+L**: Clear screen (better than `clear`)
4. **Ctrl+A/E**: Jump to start/end of line
5. **Ctrl+U/K**: Cut to start/end of line
6. **!!**: Repeat last command
7. **sudo !!**: Repeat last command with sudo
8. **cd -**: Return to previous directory
9. **Use aliases**: Save typing for common commands
10. **Learn pipe**: Combine commands with |

**Example Pipeline:**
```shell
cat access.log | grep "404" | awk '{print $1}' | sort | uniq -c | sort -rn | head -10
# Find top 10 IPs with 404 errors
```

---

## Common Pitfalls to Avoid

❌ **Don't:**
- `rm -rf /` or `rm -rf /*` (destroys system)
- `chmod 777 file` on everything (security risk)
- Forget to quote variables: `rm $file` → use `rm "$file"`
- Use `ls` output in scripts (unreliable, use `find` instead)
- Run untested scripts with sudo

✅ **Do:**
- Always backup before major changes
- Test destructive commands with `echo` first
- Use `set -euo pipefail` in scripts
- Quote variables to handle spaces
- Read man pages: `man command`

---

## Resources

- **Man Pages**: `man bash`, `man command`
- **Help**: `command --help`
- **ShellCheck**: shellcheck.net (validate scripts)
- **Explain Shell**: explainshell.com (explain commands)
- **TLDR Pages**: tldr.sh (simplified examples)

---

**🎉 You now have a complete Bash reference!**

**Bookmark this guide and refer back whenever needed. Happy scripting! 🚀**
