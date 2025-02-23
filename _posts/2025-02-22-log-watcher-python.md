---
title: Python log folder watcher
date: 2025-02-22 14:40:00 -700
categories: [homelab,software]
tags: [servers,ubuntu,linux,scripting,python]
---


Here's the complete solution in Markdown format:

```markdown
# Log File Monitor with Systemd Service

## Python Script (log_monitor.py)

```python
import os
import time
from watchdog.observers import Observer
from watchdog.events import FileSystemEventHandler
import requests

class LogFileHandler(FileSystemEventHandler):
    def __init__(self, ntfy_topic):
        self.ntfy_topic = ntfy_topic
        self.last_modified = {}

    def notify(self, filepath):
        filename = os.path.basename(filepath)
        message = f"Log file updated: {filename}"

        try:
            requests.post(
                f"https://ntfy.sh/{self.ntfy_topic}",
                data=message.encode('utf-8'),
                headers={
                    "Title": "Log File Notification",
                    "Priority": "default"
                }
            )
            print(f"Notification sent for {filename}")
        except Exception as e:
            print(f"Failed to send notification: {e}")

    def on_created(self, event):
        if event.is_directory:
            return

        filepath = event.src_path
        if filepath.endswith('.log'):
            self.notify(filepath)
            self.last_modified[filepath] = os.path.getmtime(filepath)

    def on_modified(self, event):
        if event.is_directory:
            return

        filepath = event.src_path
        if filepath.endswith('.log'):
            current_mtime = os.path.getmtime(filepath)
            last_mtime = self.last_modified.get(filepath, 0)

            if current_mtime > last_mtime:
                self.notify(filepath)
                self.last_modified[filepath] = current_mtime

def monitor_logs(directory_path, ntfy_topic):
    if not os.path.exists(directory_path):
        print(f"Directory {directory_path} does not exist")
        return

    event_handler = LogFileHandler(ntfy_topic)
    observer = Observer()
    observer.schedule(event_handler, directory_path, recursive=False)
    observer.start()

    print(f"Monitoring {directory_path} for .log files...")

    try:
        while True:
            time.sleep(1)
    except KeyboardInterrupt:
        observer.stop()
        print("\nMonitoring stopped")

    observer.join()

if __name__ == "__main__":
    WATCH_DIRECTORY = "/path/to/your/logs"  # Replace with your directory path
    NTFY_TOPIC = "your_ntfy_topic"          # Replace with your ntfy.sh topic

    monitor_logs(WATCH_DIRECTORY, NTFY_TOPIC)
```

## Setup Instructions

1. Install dependencies:
```bash
pip install watchdog requests
```

2. Save the script as `log_monitor.py` in `/usr/local/bin/`

3. Make it executable:
```bash
chmod +x /usr/local/bin/log_monitor.py
```

4. Configure the script:
- Replace `WATCH_DIRECTORY` with your directory path
- Replace `NTFY_TOPIC` with your ntfy.sh topic

## Systemd Service Configuration

1. Create service file:
```bash
sudo nano /etc/systemd/system/log-monitor.service
```

2. Add this content:
```ini
[Unit]
Description=Log File Monitor Service
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/python3 /usr/local/bin/log_monitor.py
WorkingDirectory=/usr/local/bin
Restart=always
RestartSec=10
User=your_username
Environment="PYTHONUNBUFFERED=1"

[Install]
WantedBy=multi-user.target
```

3. Replace:
- `your_username` with your actual username
- Adjust paths if different

## Service Management

1. Enable and start:
```bash
sudo systemctl daemon-reload
sudo systemctl enable log-monitor.service
sudo systemctl start log-monitor.service
```

2. Useful commands:
```bash
# Check status
sudo systemctl status log-monitor.service

# Stop service
sudo systemctl stop log-monitor.service

# Restart service
sudo systemctl restart log-monitor.service

# View logs
journalctl -u log-monitor.service
```

## Features
- Monitors .log files in specified directory
- Sends ntfy.sh notifications on create/modify
- Runs as a persistent service
- Auto-restarts on crash or reboot
- Real-time logging

## Prerequisites
- Python 3
- watchdog and requests packages
- ntfy.sh account/topic
- Linux system with systemd
```

To use this:
1. Copy the entire content into a file (e.g., `log_monitor_setup.md`)
2. Follow the instructions in the markdown to set everything up
3. The markdown file serves as both documentation and implementation guide

You can view this nicely formatted in any Markdown viewer or editor. All the code blocks will be properly syntax-highlighted when viewed in a Markdown-supporting application.
