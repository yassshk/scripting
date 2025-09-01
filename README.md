# 🔍 Service Check Script — Step‑by‑Step Guide

A simple Bash script to check whether a package/service is **installed** and whether its **process is running**.  
Use these steps on an **AWS EC2 instance** or your **local Linux machine**.

> Replace placeholders like `<YOUR_GITHUB_USERNAME>` and `<YOUR_REPO_NAME>` with your actual values.

---

## 1) Connect to the machine

### A. Connect to an AWS EC2 instance (SSH)
1. Make sure you have your key locally, e.g. `my-key.pem`.
2. Set safe permissions (otherwise SSH will refuse to use it):
   ```bash
   chmod 400 my-key.pem
   ```
3. SSH into the instance (choose the right username for your AMI):
   - **Amazon Linux / RHEL family**:
     ```bash
     ssh -i my-key.pem ec2-user@<EC2_PUBLIC_IP_OR_DNS>
     ```
   - **Ubuntu**:
     ```bash
     ssh -i my-key.pem ubuntu@<EC2_PUBLIC_IP_OR_DNS>
     ```

### B. On a local Linux machine
Just open **Terminal** and continue with the steps below.

---

## 2) Ensure Git is installed

Run one of the following (based on your OS family):

- **Amazon Linux 2023 / Fedora / RHEL9+ (dnf):**
  ```bash
  sudo dnf install -y git
  ```

- **RHEL / CentOS / Oracle Linux (yum):**
  ```bash
  sudo yum install -y git
  ```

- **Ubuntu / Debian (apt):**
  ```bash
  sudo apt-get update
  sudo apt-get install -y git
  ```

---

## 3) Clone your GitHub repository

```bash
git clone https://github.com/<YOUR_GITHUB_USERNAME>/<YOUR_REPO_NAME>.git
cd <YOUR_REPO_NAME>
```

> Example: `git clone https://github.com/yashkalode/service-check-script.git`

---

## 4) Make the script executable

Assuming your script file is named `check_service.sh`:

```bash
chmod +x check_service.sh
```

---

## 5) Run the script

```bash
./check_service.sh
```

You will be prompted to **enter the service/package name** (e.g., `mysql`, `nginx`, `httpd`).  
The script will report if the package appears installed (RPM-based check) and whether its process is running (via `systemctl` or `ps/pgrep`, depending on your implementation).

> If your script is interactive, just follow the prompt.  
> If you created a non‑interactive version that accepts the service name as an argument, you can run:  
> `./check_service.sh nginx`

---

## 6) Example session

```
Enter the service name to check:
nginx
✅ nginx is installed.
✅ nginx process is running.
```

---

## 7) Tips & Troubleshooting

- **Permission denied when running the script**  
  Make sure it’s executable:
  ```bash
  chmod +x check_service.sh
  ```

- **`git: command not found`**  
  Install Git using the commands in **Step 2** for your OS.

- **Service is installed but shows not running**  
  Start it (if you have `systemctl`):
  ```bash
  sudo systemctl start <service_name>
  sudo systemctl status <service_name>
  ```
  If `systemctl` isn’t available (containers/minimal images), you can check with:
  ```bash
  ps -ef | grep -v grep | grep <service_name>
  ```
  or more precisely:
  ```bash
  pgrep -x <service_binary_name> && echo "running" || echo "not running"
  ```

- **On Debian/Ubuntu machines**  
  Your installation check should use `dpkg -l | grep <name>` instead of `rpm -qa`.  
  Adjust your script accordingly or make it detect the OS family automatically.

---

## 8) Typical repo layout

```
<YOUR_REPO_NAME>/
├─ check_service.sh      # the bash script (interactive or with args)
└─ README.md             # this guide
```

---

## 9) License (optional)

Add a license file to your repo (e.g., MIT).  
Create `LICENSE` and choose a template from https://choosealicense.com/ .

---

## 10) Contributing (optional)

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/my-change`
3. Commit your changes: `git commit -m "Explain what changed"`
4. Push to your fork: `git push origin feature/my-change`
5. Open a Pull Request

---

**That’s it!** You (or anyone) can now connect to an EC2 or local machine, clone your repo, and run the script locally.
