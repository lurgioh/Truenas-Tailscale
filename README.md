# 🧠 TrueNAS Setup and Configuration (with Tailscale)

### Author: Harrison Lurgio  
**System:** Intel i5-8400 | 16 GB DDR4 | TrueNAS SCALE (latest)  
**Purpose:** Centralized NAS for file storage, backups, and secure remote access via Tailscale  

---

## 📘 Overview

This repository documents the setup of my **TrueNAS SCALE** system — a dedicated NAS running on physical hardware to store and manage all homelab and personal files.  
The build focuses on **reliability**, **easy access**, and **remote management** without exposing ports publicly.

---

## 🧰 Hardware Specifications

| Component | Details |
|------------|----------|
| CPU | Intel Core i5-8400 (6 cores / 6 threads) |
| RAM | 16 GB DDR4 |
| Boot Drive | 120 GB SATA SSD |
| Storage Drives | 2 × 1 TB HDD (ZFS Mirror) |
| Network | Gigabit Ethernet |
| System | HP ProDesk Mini Tower |

---

## ⚙️ Installation Process

### 1. Download TrueNAS
1. Go to [truenas.com/download-truenas-scale](https://www.truenas.com/download-truenas-scale/)
2. Choose the **TrueNAS SCALE ISO**
3. Use **BalenaEtcher** or **Rufus** to flash the ISO to a USB drive (8 GB+)

### 2. BIOS Configuration
- Set **AHCI** mode for SATA
- Disable **Secure Boot**
- Set USB as **first boot device**
- Save and reboot to USB

### 3. Installation
1. Boot from the USB installer  
2. Choose **Install/Upgrade**
3. Select SSD as the boot drive  
4. Set a **root password**
5. After install, remove the USB and reboot  
6. Note the IP address shown on screen (used for web access)

---

## 🌐 Network Setup

| Setting | Example |
|----------|----------|
| Interface | `enp3s0` |
| IP Address | `192.168.20.10` |
| Gateway | `192.168.20.1` |
| DNS | `1.1.1.1`, `8.8.8.8` |

Access the TrueNAS web interface:  
👉 **`http://192.168.20.10`**

---

## 💾 Storage Configuration

### 1. Create a ZFS Pool
- Go to **Storage → Pools → Create Pool**
- Select both HDDs  
- Choose **Mirror**
- Name it something like `tank`

### 2. Create Datasets
| Dataset | Purpose | Access |
|----------|----------|--------|
| `backups` | Backup files | SMB |
| `media` | Personal media | SMB |
| `projects` | Documents & configurations | SMB |

### 3. SMB Sharing
1. Navigate to **Sharing → Windows (SMB) Shares → Add**  
2. Choose dataset (e.g., `/mnt/tank/media`)  
3. Enable **Allow Guest Access** (optional)  
4. Apply and **start the SMB service**

---

## 🔐 Remote Access with Tailscale

### 1. Install Tailscale from TrueNAS Apps
- Go to **Apps → Discover → Tailscale → Install**
- Configure with your **Tailscale account**

### 2. Authenticate
- Sign in using your browser on any device (Google, GitHub, etc.)
- Once connected, your NAS will appear in the Tailscale admin panel

### 3. Test Connectivity
From another device on Tailscale:
```bash
ping truenas-device-name
