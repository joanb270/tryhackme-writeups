# 🌐 Network Fundamentals – Writeup (TryHackMe)

## 📌 Overview

- **Platform:** TryHackMe  
- **Module:** Network Fundamentals  
- **Difficulty:** Beginner  
- **Category:** Networking / Cybersecurity Fundamentals  
- **Focus:** How computers communicate, network models, and common weaknesses  

This writeup documents the core networking concepts learned in the *Network Fundamentals* module, written with a **SOC / Blue Team** mindset. Networking knowledge is critical for understanding attacks, detections, and incident response.

---

## 🎯 Learning Objectives

By completing this module, I aimed to:

- Understand what computer networking is and why it is important
- Learn how devices communicate with each other
- Understand Local Area Networks (LANs)
- Learn the OSI model and its layers
- Understand packets and frames
- Learn how networks are extended to the internet
- Build foundational knowledge for network-based security analysis

---

## 🧠 What is Networking?

Networking is the practice of connecting devices so they can **communicate and share data**.

Examples of networked devices:
- Computers
- Servers
- Routers
- Switches
- Mobile devices
- IoT devices

### Why Networking Matters in Cybersecurity

- Most attacks occur **over networks**
- Logs, alerts, and traffic analysis rely on network knowledge
- Understanding normal traffic helps identify malicious behavior

---

## 🏠 Introduction to LAN (Local Area Network)

### What is a LAN?

A **LAN** is a network that connects devices within a **limited geographic area**, such as:
- Home networks
- Office networks
- School networks

### Common LAN Components

- **Switches** – connect devices within the LAN
- **Routers** – connect the LAN to other networks
- **Cables / Wi-Fi** – physical or wireless connections

### Security Relevance (SOC)

- Most internal attacks occur within LANs
- Misconfigured LANs can allow lateral movement
- Understanding LAN design helps detect abnormal behavior

---

## 🧱 OSI Model

The **OSI (Open Systems Interconnection) Model** is a conceptual framework that explains how data moves across a network in **seven layers**.

### OSI Layers (Top to Bottom)

1. Application  
2. Presentation  
3. Session  
4. Transport  
5. Network  
6. Data Link  
7. Physical  

### Why the OSI Model is Important

- Helps isolate where problems occur
- Used in troubleshooting and incident analysis
- Common language for networking and security professionals

### SOC Relevance

- Attacks can target different OSI layers
- Helps categorize alerts (e.g., Layer 7 web attacks vs Layer 3 network attacks)

---

## 📦 Packets & Frames

### What are Packets?

A **packet** is a small unit of data sent across a network. Large data is broken into packets for efficient transmission.

### What are Frames?

A **frame** is the data unit used at the **Data Link layer** (Layer 2). Frames encapsulate packets for local network transmission.

### Why This Matters

- Data is not sent as one large block
- Packet inspection is key in security monitoring
- Network tools analyze packets and frames to detect threats

---

## 🌍 Extending Your Network

### Why Networks Are Extended

Networks are extended to:
- Access the internet
- Connect remote offices
- Enable cloud services

### Common Technologies

- **Routers**
- **Firewalls**
- **NAT (Network Address Translation)**
- **VPNs**

### Security Relevance

- Each extension increases the attack surface
- Misconfigurations can expose internal systems
- Firewalls and segmentation are critical controls

---

## 🧠 Key Takeaways (SOC-Focused)

- Networking is the foundation of cybersecurity
- LAN knowledge helps understand internal threats
- The OSI model provides a structured way to analyze problems
- Packets and frames explain how data actually moves
- Extending networks introduces both functionality and risk

---

## 🏁 Conclusion

The *Network Fundamentals* module establishes the baseline networking knowledge required for cybersecurity roles. Understanding how data flows across networks is essential for SOC analysis, threat detection, and incident response.

This module prepares the ground for deeper topics such as traffic analysis, firewalls, IDS/IPS, and SIEM detection logic.

---

## 🚀 Next Steps

- Network Services
- Traffic Analysis
- Firewalls and IDS/IPS
- SOC Level 1 Path

---

## ⚠️ Disclaimer

This writeup is for educational purposes only and focuses on understanding networking concepts from a defensive (SOC/Blue Team) perspective.
