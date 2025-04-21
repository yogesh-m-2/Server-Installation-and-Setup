# Server-Installation-and-Setup

🔧 1. Disable Sleep or Suspend
By default, servers shouldn't sleep, but it's good to double-check:

For servers with GUI (like Ubuntu Desktop):
Open Settings > Power.

Set Blank screen to Never.

Set Automatic suspend to Off.

For Ubuntu Server (headless / no GUI):
Check systemd settings:

bash
Copy
Edit
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
This disables all sleep/hibernate modes.

🔌 2. Ensure Power Management Settings Are Clean
Edit the logind.conf file:

bash
Copy
Edit
sudo nano /etc/systemd/logind.conf
Uncomment and change these lines:

ini
Copy
Edit
HandleLidSwitch=ignore
HandleLidSwitchDocked=ignore
HandlePowerKey=ignore
Then restart the service:

bash
Copy
Edit
sudo systemctl restart systemd-logind
Useful for laptops turned servers or devices with accidental sleep triggers.

🧠 3. Prevent Accidental Shutdown via Command Line
If you want to block non-admin users from shutting down the server:

Remove shutdown permissions from users:

bash
Copy
Edit
sudo chmod 700 /sbin/shutdown
🛡️ 4. Setup System Monitoring / Uptime Monitoring
Consider setting up monitoring with tools like:

uptime (built-in)

monit or supervisord for app monitoring

External services: UptimeRobot, Pingdom

✅ Bonus Tip: Enable Auto-Start for Your App on Boot
If your app stops due to reboot or crash, make sure it auto-starts:

Create a systemd service:

bash
Copy
Edit
sudo nano /etc/systemd/system/myapp.service
Example:

ini
Copy
Edit
[Unit]
Description=My Application
After=network.target

[Service]
ExecStart=/usr/bin/python3 /path/to/your/app.py
Restart=always
User=youruser

[Install]
WantedBy=multi-user.target
Enable it:

bash
Copy
Edit
sudo systemctl enable myapp
sudo systemctl start myapp
