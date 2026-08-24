# Cyber-Guard-GP
CyberGuard — IoT Honeypot for Attacker Behavior Analysis
CyberGuard is an IoT honeypot system built on a Raspberry Pi 3 B+ with a simulated security camera. It attracts, logs, and analyzes attacker behavior on IoT devices, giving security operators real-time visibility into intrusion attempts through a live dashboard.
📌 Overview
IoT devices — smart cameras, wearables, smart home hubs — are booming, but most ship with weak security. Their limited resources and lack of a unified security standard make them easy targets for attackers. CyberGuard addresses this by deploying a honeypot that mimics a real IoT security camera, capturing attacker tactics, techniques, and procedures (TTPs) to help security teams build better defenses.
Keywords: Honeypot, IoT, Security Operators
🎯 Aim & Objectives
Aim: Build a honeypot that detects and captures intrusion attempts against a simulated camera IoT device, then collects, displays, and monitors the resulting logs.
Objectives:
Deploy Conpot on a Raspberry Pi using a camera as the emulated IoT device
Construct a simulated network mimicking a real-world IoT deployment (routers, switches, firewalls)
Run attack simulations from Kali Linux
Provide alerting and reporting features to notify security operators of potential threats
Analyze and prioritize attack patterns
🧩 Problem Definition
Most IoT devices lack built-in security features, exposing them to attacks such as information leakage, weak authentication, and denial-of-service (DoS). Security operators struggle to detect and respond to these threats effectively. CyberGuard simulates the functional behavior and vulnerabilities of a real IoT camera as closely as possible, so the data gathered from attacker interactions is representative and useful for research and defense.
🏗️ System Architecture
The system has two main components:
Component	Details
Hardware	Raspberry Pi 3 B+, Arducam 5MP OV5647 camera
Software	Raspberry Pi OS (64-bit), Conpot
Modular Decomposition
Input Processing — Captures traffic on ports 80 (HTTP), 554 (RTSP), 22 (SSH), and 8080 (Conpot); parses protocols (HTTP, RTSP, SSH) including brute-force attempts
Output Processing — Logs attack metadata (timestamp, source IP, payload) to InfluxDB; visualizes real-time attacks and traffic via the Cockpit dashboard
System Processing — Enriches logs with GeoIP data (country/city) via geoip2; detects attack patterns (e.g., command injection, brute force) using regex
System Flow
Power on the Raspberry Pi 3 B+ and initialize Raspberry Pi OS
Start Conpot and capture network traffic
Emulate IoT camera services, capturing images/video to SD card
Start Cockpit to monitor performance and display logs
Analyze traffic → detect attacks → log suspicious activity → trigger alerts → visualize in Cockpit
⚙️ Requirements
Functional
Monitor all activity in real time
Collect logs on all interactions
Identify potential threats
Analyze collected data
Alert security operators of potential threats
Support incident-response workflows
Filter logs by priority
Support multiple users/teams
Display network logs
Non-Functional
Performance: Real-time collection and analysis with minimal latency
Reliability: Consistent, accurate detection and logging
Security: Log attacks without compromising the Raspberry Pi itself
Usability: User-friendly interface for security operators
Flexibility: Easy to adapt to new or changing threats
🛠️ Hardware
Component	Purpose
Raspberry Pi 3 B+	Core controller (Quad-core 1.2GHz, 1GB RAM, Wi-Fi/BLE, CSI camera port)
Arducam 5MP OV5647 Camera	Simulates a real IP security camera (1080p video, 2592×1944 stills)
32-bit SD Card	OS and log/config storage
Micro USB Adapter Charger (UK Plug, 5V)	Power supply
💻 Software
Raspberry Pi OS (64-bit) — Debian-based OS optimized for the Pi
Conpot — Open-source ICS/IoT honeypot framework used to emulate the camera and capture attacker interactions
Python — Primary scripting language (Conpot is Python-based); also used with Scapy/Metasploit for simulated attacks
Cockpit — Dashboard for real-time log visualization, filtering, and control (start/stop/restart Conpot)
Why Conpot over T-Pot
Feature	Conpot	T-Pot
Optimized for Raspberry Pi 3B	✅	❌ (too heavy)
IoT & SCADA Support	✅	Mostly general-purpose
Modbus, SNMP, HTTP Support	✅	Limited for IoT
Easy Integration with Cockpit	✅	Uses ELK Stack (heavy)
Customizable for Camera Simulation	✅	Harder to configure
Resource Consumption	Low	High
🚀 Installation & Deployment
bash
# Update the system
sudo apt update
sudo apt upgrade -y

# Enable the camera interface
sudo raspi-config

# Clone Conpot and install dependencies
git clone https://github.com/mushorg/conpot.git
cd conpot
pip3 install -r requirements.txt

# Create and activate a virtual environment
python3 -m venv conpot-env
source conpot-env/bin/activate
Flash Raspberry Pi OS to the SD card and boot the Pi.
Connect the Arducam 5MP OV5647 camera via the CSI port and enable it in raspi-config.
Install and configure Conpot as shown above.
Install and connect Cockpit for real-time monitoring.
🧪 Testing
Kali Linux attacks: Used nmap for reconnaissance and hping to flood the system, confirming port 22 was open and detectable.
Conpot setup test: Verified Conpot installs and runs without errors (resolved a Python version incompatibility by installing an older, compatible Python version).
End-to-end test: Ran nmap -sS <target-ip> against the honeypot and confirmed the attack was logged and visualized in Cockpit.
Results
The system successfully logged network activity and responded to simulated attacks. The nmap scan identified open ports, and the hping flood simulated a DoS attack that triggered abnormal traffic logging. Conpot–Cockpit integration provided real-time attack data for monitoring and analysis, after resolving initial configuration/compatibility issues between components.
🔮 Future Work
Improve scalability with more advanced honeypot solutions
Integrate AI-based detection to automate threat identification
Expand beyond Conpot's ICS-focused templates toward more IoT-specific device emulation
📚 References
See the full graduation project report for the complete literature review and reference list, including works on hybrid IoT/OT honeypots, adaptive deception frameworks, zero-day detection architectures, and IoT malware forensics (Srinivasa et al., Tagliaro et al., Putrevu et al., Ellouh et al., and others).
