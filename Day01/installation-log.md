# Installation Log – Docker Desktop on Windows 11

> **Project:** Docker Learning Journey – Day 1  
> **Environment:** Windows 11 Pro (21H2)  
> **Docker Backend:** WSL2  
> **Status:** ✅ Successfully Installed

---

# Purpose

This document records the installation process, challenges encountered, root causes, troubleshooting steps, and final verification of Docker Desktop on Windows 11.

Rather than documenting only the successful installation, this log captures the real-world issues encountered during setup and how each one was diagnosed and resolved. It serves as both a personal reference and evidence of practical problem-solving during my Docker learning journey.

---

# System Specifications

| Component | Details |
|-----------|---------|
| Operating System | Windows 11 Pro (21H2) |
| Processor | Intel Core i3-6100U |
| Memory | 8 GB RAM |
| Docker Backend | WSL2 |
| Linux Distribution | Ubuntu 24.04 |
| Docker Desktop | Installed Successfully |

---

# Installation Timeline

The installation followed these major steps:

1. Installed Docker Desktop.
2. Enabled Windows Subsystem for Linux (WSL2).
3. Enabled hardware virtualization in BIOS.
4. Configured Docker Desktop to use the WSL2 backend.
5. Verified Docker Engine.
6. Pulled and executed the **hello-world** container.

During this process, four installation challenges were encountered.

---

# Challenge 1 – WSL2 Failed to Start

## Error

```text
WSL2 is unable to start since virtualization is not enabled on this machine.

Please ensure the "Virtual Machine Platform"
optional component is enabled and virtualization
is turned on in your computer's firmware settings.
```

---

## Root Cause

WSL2 requires hardware-assisted virtualization (Intel VT-x / AMD-V).

Although Docker Desktop was installed correctly, virtualization was disabled in the BIOS, preventing WSL2 from starting.

---

## Investigation

To verify the issue, I confirmed that:

- Docker Desktop was installed.
- WSL components were present.
- Windows Virtual Machine Platform was available.

The remaining missing requirement was hardware virtualization.

---

## Resolution

I restarted the computer and entered the BIOS setup.

Steps performed:

- Navigated to BIOS/UEFI Settings.
- Located **Virtualization Technology (VT-x)**.
- Changed the setting from **Disabled** to **Enabled**.
- Saved changes and rebooted Windows.

---

## Outcome

WSL2 successfully initialized after reboot.

---

# Challenge 2 – Secure Boot Authorization

## What Happened

Immediately after enabling virtualization in BIOS, the system displayed a Secure Boot authorization screen requesting a numeric verification code.

---

## Root Cause

This behavior is a security feature implemented by many manufacturers.

Firmware changes affecting security settings require manual confirmation to prevent unauthorized software from modifying BIOS settings.

---

## Resolution

- Entered the displayed authorization code.
- Confirmed the firmware change.
- Continued the boot process.

---

## Outcome

Windows started normally and BIOS changes were successfully applied.

---

# Challenge 3 – PowerShell Execution Policy

## Error

```text
profile.ps1 cannot be loaded because running scripts is disabled on this system.
```

---

## Root Cause

Windows PowerShell uses a **Restricted Execution Policy** by default.

This security policy blocks local PowerShell profile scripts from running.

Importantly, this issue was unrelated to Docker or WSL.

---

## Investigation

To confirm Docker was unaffected, I executed:

```powershell
wsl --status
```

The command returned:

```text
Default Distribution: Ubuntu-24.04
Default Version: 2
```

This confirmed WSL was functioning correctly.

---

## Resolution

Optionally updated the execution policy:

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

Although not required for Docker, this removes the profile script warning for future PowerShell sessions.

---

## Outcome

PowerShell continued functioning normally and Docker remained unaffected.

---

# Challenge 4 – Docker Daemon Connection

## Error

```text
error during connect:

The system cannot find the file specified.
```

---

## Root Cause

Docker Desktop had not fully started.

The Docker CLI attempted to communicate with the Docker Engine before the Docker daemon had finished initializing.

---

## Investigation

Verified that:

- Docker Desktop was still loading.
- Docker Engine status had not yet changed to **Running**.

---

## Resolution

Waited for Docker Desktop to finish initialization before rerunning Docker commands.

---

## Outcome

Once Docker Engine reported **Running**, all Docker commands executed successfully.

---

# Final Verification

After resolving the installation issues, I verified the Docker environment using:

```powershell
docker run hello-world
```

Docker completed the following workflow successfully:

1. Connected to Docker Hub.
2. Downloaded the Hello World image.
3. Created a container.
4. Started the container.
5. Executed the application.
6. Displayed the expected success message.

This confirmed that:

- Docker Client was working.
- Docker Engine was running.
- WSL2 backend was configured correctly.
- Image downloads were successful.
- Container execution was functioning correctly.

---

# Configuration Verification

The following configuration was confirmed after installation.

| Component | Status |
|-----------|--------|
| Docker Desktop Installed | ✅ |
| Docker Engine Running | ✅ |
| WSL2 Backend Enabled | ✅ |
| Ubuntu 24.04 Installed | ✅ |
| Docker CLI Working | ✅ |
| Hello World Container Executed | ✅ |

---

# Lessons Learned

This installation highlighted that Docker Desktop depends on several underlying technologies working together.

Key takeaways include:

- Docker Desktop on Windows relies on WSL2 as its Linux backend.
- WSL2 requires hardware virtualization to be enabled in BIOS.
- Docker commands communicate with the Docker Engine, so the engine must be fully initialized before running commands.
- Some Windows security features, such as PowerShell execution policies and Secure Boot, may appear during installation but are separate from Docker itself.

Understanding these dependencies provided a deeper appreciation of Docker's architecture beyond simply completing the installation.

---

# Reflection

One of the most valuable parts of Day 1 was the troubleshooting process itself.

Rather than following an installation guide without issues, I encountered several real-world configuration challenges that required investigation and problem-solving.

Working through BIOS settings, WSL2 configuration, Windows security policies, and Docker Engine initialization gave me a much stronger understanding of how Docker operates on Windows.

The experience reinforced an important lesson:

> Installing software is one skill.

> Understanding *why* it works—and why it sometimes doesn't—is the foundation of becoming an effective engineer.

---

# Conclusion

Docker Desktop was successfully installed and configured using the WSL2 backend on Windows 11.

All installation objectives were achieved, and the environment is now ready for subsequent lessons covering Docker Images, Containers, Networking, Volumes, and Docker Compose.

**Day 1 Status:** ✅ Complete