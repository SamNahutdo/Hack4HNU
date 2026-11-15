<div align="center">

  <img src="hnu-logo.png" alt="Holy Name University Logo" width="120" />

  # HNU CTF PLATFORM
  ### Cybersecurity Challenge & SOC Telemetry Monitoring System

  <p align="center">
    A high-performance, self-hosted <b>Capture The Flag (CTF)</b> and <b>Security Operations Center (SOC)</b> platform engineered for offline, local area network (LAN), and classroom cybersecurity competitions.
  </p>

  <!-- Badges -->
  <p align="center">
    <img src="https://img.shields.io/badge/PHP-8.1%2B-777BB4?style=for-the-badge&logo=php&logoColor=white" alt="PHP" />
    <img src="https://img.shields.io/badge/React-18.x-61DAFB?style=for-the-badge&logo=react&logoColor=black" alt="React" />
    <img src="https://img.shields.io/badge/Vite-6.x-646CFF?style=for-the-badge&logo=vite&logoColor=white" alt="Vite" />
    <img src="https://img.shields.io/badge/TailwindCSS-3.x-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white" alt="Tailwind" />
    <img src="https://img.shields.io/badge/MySQL-MariaDB-4479A1?style=for-the-badge&logo=mysql&logoColor=white" alt="MySQL" />
    <img src="https://img.shields.io/badge/OS-Windows_CMD-0078D6?style=for-the-badge&logo=windows&logoColor=white" alt="Windows" />
  </p>

  <p align="center">
    <a href="#features">Features</a> •
    <a href="#project-directory-structure">Project Structure</a> •
    <a href="#system-prerequisites">Prerequisites</a> •
    <a href="#installation--setup-guide">Installation</a> •
    <a href="#running-the-platform-windows-cmd">Execution Guide</a> •
    <a href="#competition-rules--regulations">Rules</a> •
    <a href="#how-to-solve-challenges">Challenge Guide</a>
  </p>

</div>

---

## Table of Contents

- [Platform Overview](#platform-overview)
- [Key Features](#key-features)
- [Project Directory Structure](#project-directory-structure)
- [System Prerequisites](#system-prerequisites)
- [Installation & Setup Guide](#installation--setup-guide)
  - [Step 1: Database Provisioning (XAMPP / MySQL)](#step-1-database-provisioning-xampp--mysql)
  - [Step 2: Database Configuration](#step-2-database-configuration)
  - [Step 3: Frontend Dependency Installation](#step-3-frontend-dependency-installation)
- [Running the Platform (Windows CMD)](#running-the-platform-windows-cmd)
  - [Terminal 1: Start PHP Backend](#terminal-1-start-php-backend)
  - [Terminal 2: Start React Frontend](#terminal-2-start-react-frontend)
  - [Network URLs](#network-access-urls)
- [Competition Rules & Regulations](#competition-rules--regulations)
- [How to Solve Challenges (Flag Format)](#how-to-solve-challenges)
- [User Roles & Permissions](#user-roles--permissions)
- [Troubleshooting](#troubleshooting)

---

## Platform Overview

The **HNU CTF Platform** is designed for educational cybersecurity competitions, workshops, and hackathons at **Holy Name University**. Built with zero external cloud dependencies, it functions entirely offline within a local Wi-Fi router, lab switch, or localhost environment while offering enterprise-grade SOC attack monitoring and anti-cheat telemetry.

```text
                    +----------------------------------------+
                    |      HOST PC (Administrator / Server)  |
                    |  - PHP API Backend (:8000)             |
                    |  - MySQL Database (3306 / 3308)        |
                    |  - Vite / Static Frontend (:5173)      |
                    +-------------------┬--------------------+
                                        | (Local Wi-Fi / LAN)
           +----------------------------+----------------------------+
           |                                                         |
           v                                                         v
+-----------------------+                                 +-----------------------+
| Participant Team 1    |                                 | Participant Team 2    |
| http://<HOST_IP>:5173 |                                 | http://<HOST_IP>:5173 |
+-----------------------+                                 +-----------------------+
```

---

## Key Features

### For Participants
* **Categorized Challenge Hub**: Challenge grid categorized into Web Exploitation, OSINT, Network Forensics, Digital Forensics, Steganography, and Miscellaneous.
* **Isolated Scoreboard View**: Standings remain strictly confidential during competition to preserve competitive integrity; teams see only their own members' verified solves and scores.
* **Synchronized Competition Clock**: Live real-time synchronized timer indicating contest state (Live Countdown, Paused, or Concluded).
* **Protected File Deliveries**: Challenge artifacts (.pcap, .zip, .png, scripts) are delivered exclusively through authenticated PHP streaming sessions behind an `.htaccess` firewall.

### For Administrators & SOC Operators
* **SOC Telemetry & Brute-Force Detection**: Real-time detection of brute-force flag submission anomalies with configurable thresholds (1-min Burst, 5-min Sustained, 10-min Critical).
* **Automated & Manual IP Enforcement**: View suspicious client IPs, investigate request origins, and apply 1-click IP firewall bans.
* **Rule Enforcement & Participant Suspension**: Apply instant 30-minute suspensions to rule-violating accounts or entire teams. Suspended participants receive an animated blurred lock screen and are blocked from submitting flags.
* **Live Roster & Team Management**: Provision and manage 4-member teams, reveal credentials, delete teams or individual accounts, and broadcast event announcements with unread notification badges.

---

## Project Directory Structure

```text
HACKSAHNU/
|-- backend/                               # Native PHP REST API Engine
|   |-- api/                               # Core API Endpoints
|   |   |-- accounts.php                   # Account provisioning, teams, delete & suspension
|   |   |-- announcements.php              # Event bulletins & notifications
|   |   |-- auth.php                       # Authentication & session verification
|   |   |-- challenges.php                 # Challenge explorer & category catalog
|   |   |-- dashboard.php                  # Participant metrics & team analytics
|   |   |-- download.php                   # Secure attachment download streamer
|   |   |-- leaderboard.php                # Scoring engine (Isolated vs Full Admin)
|   |   |-- monitoring.php                 # SOC attack metrics & brute-force rules
|   |   |-- submissions.php                # Flag submission & audit recording
|   |   `-- timer.php                      # Authoritative competition timer sync
|   |-- config/
|   |   `-- database.php                   # MySQL PDO connection with auto-port fallback
|   |-- includes/
|   |   |-- audit_logger.php               # Security incident & audit event logger
|   |   |-- brute_force_detection.php      # Real-time brute force detection engine
|   |   `-- flag_verification.php          # Constant-time flag comparison & scoring
|   `-- middleware/
|       |-- admin.php                      # Administrator authorization guard
|       `-- auth.php                       # Session authentication & CORS headers
|-- frontend/                              # React 18 Single Page Application
|   |-- dist/                              # Pre-compiled production web assets
|   |-- src/
|   |   |-- components/                    # Reusable UI components (Navbar, Modals, Screens)
|   |   |-- hooks/                         # useCompetitionTimer hook (live socket/poll sync)
|   |   |-- pages/                         # Application Views
|   |   |   |-- AccountsPage.jsx           # Admin account, team roster & suspension manager
|   |   |   |-- AdminDashboard.jsx         # Admin competition controls & team overview
|   |   |   |-- AnnouncementsPage.jsx      # Bulletin board & broadcast releases
|   |   |   |-- ChallengeExplorer.jsx      # Participant challenge hub & search
|   |   |   |-- LeaderboardPage.jsx        # Team rankings & participant contributions
|   |   |   |-- LoginPage.jsx              # Secure dark-mode login interface
|   |   |   |-- ParticipantDashboard.jsx   # Hero timer, team stats & analytics charts
|   |   |   `-- SystemMonitorPage.jsx      # SOC telemetry & brute-force firewall controls
|   |   |-- services/
|   |   |   `-- api.js                     # Unified API client service
|   |   |-- App.jsx                        # Root React component & role router
|   |   `-- main.jsx                       # Application entry point
|   |-- package.json                       # Frontend dependencies & npm scripts
|   `-- vite.config.js                     # Vite build configuration & reverse proxy
|-- documentation/                         # Schemas & Deployment Documentation
|   |-- database_schema.sql                # Full MySQL schema & authentic seed records
|   |-- database_schema.md                 # Table relationships & schema reference
|   `-- installation.md                    # Installation documentation
|-- storage/                               # Protected Competition Storage
|   |-- challenges/                        # Authentic challenge attachments & artifacts
|   |-- timer_state.json                   # Offline fallback timer persistence
|   `-- .htaccess                          # Security policy blocking direct URL file access
|-- router.php                             # Unified PHP built-in server router
`-- README.md                              # Project documentation
```

---

## System Prerequisites

Ensure the following tools are installed on the **Host Windows PC**:

| Requirement | Recommended Version | Download Link |
| :--- | :--- | :--- |
| **Windows OS** | Windows 10 / 11 (64-bit) | Included with PC |
| **XAMPP** | Apache & MySQL (PHP 8.1+) | [apachefriends.org](https://www.apachefriends.org/) |
| **Node.js** | v18.x or v20.x LTS with npm | [nodejs.org](https://nodejs.org/) |
| **Web Browser** | Chrome, Edge, Firefox, or Brave | Modern standard browser |

---

## Installation & Setup Guide

> **Note**: Perform all commands using the native **Windows Command Prompt (`cmd.exe`)**.

### Step 1: Database Provisioning (XAMPP / MySQL)

1. Open the **XAMPP Control Panel**.
2. Click **Start** next to **MySQL** (and **Apache** if using phpMyAdmin).
3. Open your browser and navigate to **phpMyAdmin**:
   ```text
   http://localhost/phpmyadmin
   ```
4. Click **New** in the left sidebar and create a database named:
   * **Database Name**: `hnu_ctf`
   * **Collation**: `utf8mb4_unicode_ci`
5. Select `hnu_ctf`, click the **Import** tab at the top.
6. Click **Choose File** and select:
   ```text
   C:\HACKSAHNU\documentation\database_schema.sql
   ```
7. Click **Import** at the bottom to execute the schema.

---

### Step 2: Database Configuration

The backend auto-detects standard XAMPP configurations (supporting both default port `3306` and custom port `3308`). 

To verify or change connection settings, open:  
`c:\Users\Sam Nahutdo\Documents\HACKSAHNU\backend\config\database.php`

```php
define('DB_HOST', '127.0.0.1');
define('DB_PORT', '3308');       // Port 3308 (with automatic fallback to 3306)
define('DB_NAME', 'hnu_ctf');
define('DB_USER', 'root');
define('DB_PASS', '');           // Default is empty in XAMPP
```

---

### Step 3: Frontend Dependency Installation

1. Open **Windows Command Prompt (`cmd.exe`)**:
   * Press <kbd>Win</kbd> + <kbd>R</kbd>, type `cmd`, and press <kbd>Enter</kbd>.
2. Run the following commands to install packages and compile the production build:
   ```cmd
   cd "\HACKSAHNU\frontend"
   npm install
   npm run build
   ```

---

## Running the Platform (Windows CMD)

To run the platform during the competition with live development sync, open **two separate Command Prompt windows**.

### Terminal 1: Start PHP Backend

Open the first Windows Command Prompt:

```cmd
cd "\HACKSAHNU"
php -S 0.0.0.0:8000 router.php
```

> **Note on `0.0.0.0`**: Binding to `0.0.0.0` allows all computers and laptops connected to your Wi-Fi or LAN switch to communicate with your backend.

---

### Terminal 2: Start React Frontend

Open a second Windows Command Prompt:

```cmd
example:
cd "c:\Users\Sam Nahutdo\Documents\HACKSAHNU\frontend"
npm run dev
```

---

### Network Access URLs

Replace `<HOST_IP>` with your host PC's IPv4 address (found by running `ipconfig` in CMD, for example `192.168.1.13`):

| Device | Purpose | Address |
| :--- | :--- | :--- |
| **Host PC (Admin)** | Administrator Portal | `http://localhost:5173` |
| **Participants** | Competition Dashboard | `http://<HOST_IP>:5173` |
| **Direct Production** | Unified Router (Optional) | `http://<HOST_IP>:8000` |

---

## Competition Rules & Regulations

All participants must abide by Holy Name University event rules:

1. **Team Composition**:
   - Each registered team consists of strictly **4 active participants**.
   - Individual member points aggregate dynamically toward the team's total standing.
2. **Confidential Standings**:
   - In accordance with competition rules, competitor scores and global rankings are **isolated and confidential** during the active contest. Participants can only see their own team's solved challenges and points.
3. **Strict Flag Format**:
   - All submitted flags must follow the official flag format (see below). Flags are case-sensitive and must be submitted exactly as found.
4. **Anti-Brute-Force & Rate Limiting**:
   - Automated fuzzing or submitting flags using automated brute-force scripts is monitored by the active SOC firewall. Repeated failed submissions will trigger rate-limits and temporary IP lockouts.
5. **30-Minute Suspension Rule**:
   - Violating platform rules (such as attempting Denial of Service, attacking competition infrastructure, unauthorized credential tampering, or cross-team collusion) will result in an immediate **30-minute account/team suspension** administered through the Admin Panel.
   - During suspension, the interface blurs with a countdown lock screen and all flag submissions are rejected.
6. **No Infrastructure Attacks**:
   - Do **not** attack the CTF scoring server or host machine. Challenges are designed to be solved through analysis, inspection, and reverse engineering.

---

## How to Solve Challenges

The challenge repository covers various offensive and defensive cybersecurity topics. **Challenge titles, descriptions, and answers are kept confidential.**

### Flag Submission Format

Every valid challenge flag follows the standard format:

```text
HNU{flag_text_goes_here}
```

* **Prefix**: Always starts with `HNU{` (uppercase).
* **Suffix**: Always ends with `}`.
* **Inner Text**: May contain alphanumeric characters, underscores, leetspeak, or special symbols (for example `HNU{sample_flag_2026}`).

### Solving Workflow

```text
+------------------+     +-----------------------+     +----------------------+
| 1. Browse Hub    | --> | 2. Inspect & Download | --> | 3. Analyze Artifacts |
| Select category  |     | Read narrative prompt |     | Wireshark, CyberChef,|
| and point value  |     | & get protected files |     | Python, Stegseek     |
+------------------+     +-----------------------+     +----------┬-----------+
                                                                  |
                                                                  v
+------------------+     +-----------------------+     +----------------------+
| 6. Earn Points!  | <-- | 5. Verification Match | <-- | 4. Submit Flag       |
| Team total rises |     | Constant-time match   |     | Format: HNU{...}     |
| & solve recorded |     | prevents timing leak  |     | Instant live feedback|
+------------------+     +-----------------------+     +----------------------+
```

1. **Access the Challenge Hub**: Open the **Challenges** navigation tab.
2. **Review the Briefing**: Click on any challenge card to open the inspection modal.
3. **Download Attachments**: If the challenge includes artifacts (such as `.pcap`, `.zip`, `.png`, or `.py`), click **Download Attachment**.
4. **Local Investigation**: Analyze the artifact locally using standard cybersecurity tools:
   * **Network**: Inspect packet traffic with Wireshark or tshark.
   * **Forensics & Stego**: Analyze images with CyberChef, binwalk, zsteg, or strings.
   * **Web & OSINT**: Investigate hints, target web ports, headers, and recon clues.
5. **Submit & Score**: Enter your discovery into the flag box and press **Submit Flag**. Correct flags immediately credit points to your team.

---

## User Roles & Permissions

Public self-registration is strictly disabled to prevent unauthorized accounts during the event. Accounts are provisioned in advance by administrators.

| Role | Access Permissions | Interface |
| :--- | :--- | :--- |
| **Administrator** | Full system control, live timer broadcast, 30m suspension controls, team roster management, SOC attack monitoring, and announcements | Admin Dashboard & SOC Console |
| **Participant** | Challenge hub, flag submission, team contribution view, isolated team analytics, and official bulletins | Participant Dashboard |

---

## Troubleshooting

<details>
<summary><b>1. "Failed to communicate with the HNU CTF backend server"</b></summary>
<br>

* Ensure the PHP server is running in Command Prompt:
  ```cmd
  cd "c:\Users\Sam Nahutdo\Documents\HACKSAHNU"
  php -S 0.0.0.0:8000 router.php
  ```
* Ensure MySQL is running in XAMPP.
</details>

<details>
<summary><b>2. Participants Cannot Access from Other Laptops</b></summary>
<br>

* Verify all laptops are connected to the **same Wi-Fi network or switch**.
* Check your IPv4 address by running `ipconfig` in Command Prompt.
* Make sure you shared the correct IP: `http://<YOUR_IP>:5173`.
* Verify that Windows Firewall allows connections on ports `5173` and `8000`:
  1. Open Windows Defender Firewall -> Allow an app or feature through Windows Defender Firewall.
  2. Ensure **Node.js** and **PHP** have checks under **Private**.
</details>

<details>
<summary><b>3. Database Connection Refused (Ports 3306 / 3308)</b></summary>
<br>

* Open XAMPP Control Panel and check which port MySQL is using.
* The backend automatically checks both port `3308` and `3306`. If your MySQL port is customized, update `backend/config/database.php`.
</details>

---

### Developers


| Name | Role |  
| :--- | :--- | 
| Sam |  | 
| Siris | |
| Marchus |  |
| Rael |  |
| Mj |  |
| Cris |  |
| Alexes |  |

---

<div align="center">
  <sub>Holy Name University - College of Computer Studies * Cybersecurity Challenge Platform</sub>
</div>
