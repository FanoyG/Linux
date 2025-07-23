# 🚀 Linux Commands for Cloud Engineers

A comprehensive, hands-on reference guide for essential Linux commands specifically tailored for cloud infrastructure management. This repository provides practical examples, real-world use cases, and expert insights to accelerate your cloud engineering journey.

## 📋 Quick Navigation

| Day | Topic | Focus Area | Status |
|-----|-------|------------|--------|
| [Day 1](#day-1---file-system--navigation) | File System & Navigation | Basic file operations, directory management | ✅ Complete |
| [Day 2](#day-2---coming-soon) | File Content & Text Processing | `cat`, `grep`, `awk`, `sed` | 🚧 Coming Soon |
| [Day 3](#day-3---coming-soon) | Process Management | `ps`, `top`, `kill`, `systemctl` | 🚧 Coming Soon |
| [Day 4](#day-4---coming-soon) | Network & Connectivity | `curl`, `wget`, `netstat`, `ss` | 🚧 Coming Soon |
| [Day 5](#day-5---coming-soon) | Permissions & Security | `chmod`, `chown`, `sudo`, `ssh` | 🚧 Coming Soon |

---

## 🎯 How to Use This Guide

**Progressive Learning Path:** Each day builds upon previous concepts, designed for practical cloud scenarios.

**Structure:** Every command includes:
- 📖 **Definition** - Clear explanation
- ⚙️ **Syntax** - Common usage patterns  
- ☁️ **Cloud Use Case** - Real EC2/server scenarios
- 💡 **Pro Tip** - Expert insights and best practices

---

## Day 1 - File System & Navigation

Essential commands for navigating and managing files on cloud servers like EC2 instances.

### `ls` - List Directory Contents

**📖 Definition:** Display files and directories with detailed information.

```bash
ls                    # Basic listing
ls -la                # Detailed view with hidden files
ls -lh                # Human-readable file sizes
ls -lt                # Sort by modification time
```

**☁️ Cloud Use Case:** Check deployed application files after SSH into EC2
```bash
ls -la /var/www/html/     # Verify web files deployment
ls -lh /var/log/          # Check log file sizes
```

**💡 Pro Tip:** Use `ls -lah` as your default - shows permissions, sizes, and hidden config files.

---

### `cd` - Change Directory

**📖 Definition:** Navigate between directories efficiently.

```bash
cd /path/to/directory     # Absolute path
cd ..                     # Go up one level
cd ~                      # Home directory
cd -                      # Previous directory
```

**☁️ Cloud Use Case:** Navigate common cloud server locations
```bash
cd /var/log/nginx/        # Check web server logs
cd /etc/systemd/system/   # Access service configurations
cd -                      # Toggle between locations
```

**💡 Pro Tip:** Use `cd -` to quickly switch between two working directories during troubleshooting.

---

### `pwd` - Print Working Directory

**📖 Definition:** Display current directory path.

```bash
pwd
```

**☁️ Cloud Use Case:** Confirm location before running scripts or making changes
```bash
pwd && ls -la             # Verify location and contents
```

**💡 Pro Tip:** Always run `pwd` before executing potentially destructive commands.

---

### `mkdir` - Create Directories

**📖 Definition:** Create directories with proper structure.

```bash
mkdir directory_name          # Single directory
mkdir -p path/to/directory    # Create parent directories
mkdir -m 755 secure_dir       # Set permissions during creation
```

**☁️ Cloud Use Case:** Prepare application directory structure
```bash
mkdir -p /opt/app/{logs,config,data}    # Create app structure
mkdir -p /backup/$(date +%Y-%m-%d)      # Dated backup directory
```

**💡 Pro Tip:** Always use `mkdir -p` in automation scripts to prevent errors.

---

### `touch` - Create Files

**📖 Definition:** Create empty files or update timestamps.

```bash
touch filename.txt            # Create empty file
touch file1.txt file2.txt     # Multiple files
```

**☁️ Cloud Use Case:** Initialize configuration files
```bash
touch /etc/nginx/sites-available/myapp.conf
touch .env.production         # Environment file
```

**💡 Pro Tip:** Use `touch` to create placeholder files before editing with `nano` or `vim`.

---

### `cp` - Copy Files and Directories

**📖 Definition:** Copy files and directories with various options.

```bash
cp source.txt destination.txt     # Copy file
cp -r source_dir/ dest_dir/       # Copy directory recursively
cp -p file.txt backup/            # Preserve permissions and timestamps
```

**☁️ Cloud Use Case:** Backup configurations before changes
```bash
cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup
cp -r /var/www/html/ /backup/website-$(date +%Y%m%d)/
```

**💡 Pro Tip:** Use `cp -rp` to maintain file permissions and timestamps when copying directories.

---

### `mv` - Move and Rename

**📖 Definition:** Move files/directories or rename them.

```bash
mv old_name.txt new_name.txt      # Rename file
mv file.txt /path/to/directory/   # Move file
mv *.log /var/log/archive/        # Move multiple files
```

**☁️ Cloud Use Case:** Organize and archive files
```bash
mv /var/log/app.log /var/log/archive/app-$(date +%Y%m%d).log
mv temp_config.conf /etc/app/production.conf
```

**💡 Pro Tip:** Use `mv -v` for verbose output to track what's being moved in bulk operations.

---

### `rm` - Remove Files and Directories

**📖 Definition:** Delete files and directories safely.

```bash
rm filename.txt               # Remove file
rm -r directory/              # Remove directory recursively  
rm -f filename.txt            # Force remove without confirmation
```

**☁️ Cloud Use Case:** Clean up temporary files and old deployments
```bash
rm -rf /tmp/deployment-*      # Remove old deployment files
find /var/log -name "*.log" -mtime +30 -delete    # Remove old logs
```

**💡 Pro Tip:** ⚠️ **DANGER:** Never use `rm -rf /` or `rm -rf /*` - always double-check paths!

---

### `find` - Search Files and Directories

**📖 Definition:** Powerful search utility with multiple criteria.

```bash
find /path -name "pattern"           # Find by name
find /path -type f -name "*.log"     # Find files with extension
find /path -mtime +7                 # Files older than 7 days
find /path -size +100M               # Files larger than 100MB
```

**☁️ Cloud Use Case:** Locate and manage server files
```bash
find /var/log -name "*.log" -size +10M          # Large log files
find /opt -type f -name "*.conf" -mtime -1      # Recent config changes
find /home -type f -perm 777                    # Security audit
```

**💡 Pro Tip:** Combine `find` with `exec` for batch operations: `find /path -name "*.tmp" -exec rm {} \;`

---

### `locate` - Fast File Search

**📖 Definition:** Quick file search using system index.

```bash
locate filename               # Find file quickly
locate "*.conf" | head -10   # Find config files (first 10)
```

**☁️ Cloud Use Case:** Quickly locate configuration files
```bash
locate nginx.conf             # Find nginx config
locate "*.pem"               # Find SSL certificates
```

**💡 Pro Tip:** Run `sudo updatedb` weekly to keep the search index current.

---

## 🛠️ Practical Cloud Scenario

**Scenario:** You've just deployed a new application on an EC2 instance and need to set up the directory structure and verify deployment.

```bash
# 1. Check current location and deployed files
pwd && ls -la

# 2. Navigate to application directory
cd /opt/myapp

# 3. Create required directory structure
mkdir -p {logs,config,data,backup}

# 4. Verify structure was created
ls -la

# 5. Create initial configuration file
touch config/app.conf

# 6. Copy sample configuration
cp config/app.conf.sample config/app.conf

# 7. Check for any existing log files
find . -name "*.log" -type f

# 8. Verify everything is in place
pwd && ls -la */
```

---

## Day 2 - Coming Soon
File Content & Text Processing commands including `cat`, `grep`, `awk`, and `sed`.

## Day 3 - Coming Soon  
Process Management with `ps`, `top`, `kill`, and `systemctl`.

## Day 4 - Coming Soon
Network & Connectivity tools like `curl`, `wget`, `netstat`, and `ss`.

## Day 5 - Coming Soon
Permissions & Security including `chmod`, `chown`, `sudo`, and `ssh`.

---

## 🤝 Contributing

Found a useful command or have a great cloud use case? Feel free to contribute! This is a living document designed to help cloud engineers master Linux efficiently.
