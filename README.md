# 📡 Private 5G Network Setup

This repository documents the **end-to-end setup** of a real-time 5G network using:

- **Open5GS** as the 5G Core  
- **srsRAN** as the Radio Access Network (RAN)  
- **USRP B210** as the base station hardware  
- Connected to **Commercial Off-The-Shelf (COTS)** 5G devices (smartphones)  

✅ Fully tested with **Samsung S23** and **OPPO Reno 5G** using **OpenCell SIM cards**.


## 🛠 Requirements

| Component       | Purpose |
|-----------------|---------|
| **Ubuntu 22.04 LTS** | Operating system for running Open5GS & srsRAN |
| **USRP B210**   | Software-defined radio hardware acting as the 5G base station |
| **OpenCell SIM** | SIM cards for registering UEs with the 5G core |
| **COTS UE**     | 5G smartphones for testing connectivity |

💡 **Tip:** Install Ubuntu on bare metal (dual boot recommended) instead of using a virtual machine for better hardware performance and less probability of the errors.


## 🚀 Setup Guide

Follow the steps in the [`Steps-1-7`](./Steps-1-7) folder for a detailed walk-through.

### High-Level Process

1. **Open5GS Setup** – Install and configure the 5G Core network.
2. **UHD Drivers Installation** – Set up USRP B210 drivers for hardware communication.
3. **srsRAN Setup** – Configure the Radio Access Network.
4. **Open5GS WebUI User Creation** – Register UE subscribers in the 5G Core.
5. **SIM Writing** – Program OpenCell SIM cards with matching IMSI & keys.
6. **COTS UE Configuration** – Configure mobile phones for private 5G connection.
7. **Connectivity Test** – Verify registration, attach procedure, and data transfer.

---

## 🧠 Open5GS – The Core Network

Open5GS is like the **brain** of the mobile network.  
It handles device registration, authentication, IP address allocation, mobility, and data routing.

### Main Functions

- **AMF (Access and Mobility Management Function)**  
  - Think of it like airport security — it checks who’s coming in (authentication) and makes sure they stay connected as they move around.  
  - Handles UE registration, authentication, and handovers.

- **SMF (Session Management Function)**  
  - Like a traffic planner — it gives each device an IP and decides how their data will travel.  
  - Manages session creation and modifies data paths.

- **UPF (User Plane Function)**  
  - The actual data highway — where user internet traffic flows in and out.  
  - Forwards data between the UE and the internet/application servers.

- **PCF (Policy Control Function)**  
  - The rulebook — decides speed limits, QoS, and access policies for each device.  
  - Enforces policies set by the network.

- **NRF (Network Repository Function)**  
  - The phonebook — keeps track of where each function (like AMF, SMF) is running.  
  - Helps network functions discover and communicate with each other.

---

## 📡 srsRAN – The Radio Access Network

srsRAN is like the **hands and ears** of the network.  
It uses software-defined radios (SDR) like the **USRP B210** to send and receive wireless signals to the devices (UEs).

In my setup, srsRAN acted as the 5G **gNB** (base station), which is split into:

- **RU (Radio Unit)** – Sends and receives signals over the air to/from the devices.
- **DU (Distributed Unit)** – Handles real-time functions such as scheduling and MAC/RLC processing.
- **CU (Central Unit)** – Manages non-real-time functions like RRC signaling, mobility, and links the radio to the Open5GS core (via NG interfaces).

---

## 🔗 How They Work Together

1. **UE** connects to **gNB** (via srsRAN with USRP B210).  
2. gNB forwards control signaling and data to **Open5GS** core functions.  
3. **AMF** authenticates the UE, **SMF** assigns an IP, **UPF** routes the traffic, and **PCF** applies policies.  
4. **NRF** ensures all functions can find each other and work together.  

📌 **In short:**  
- **Open5GS** = network brain.  
- **srsRAN + USRP B210** = base station.  
- Together, they form a complete private 5G setup for research and testing.

---

## 🖼 Conceptual Diagram
- COTS -> Base station(Tower) -> Network CORE -> Internet
> [UE] ⇄ [USRP B210 -> srsRAN] ⇄ [Open5GS] ⇄ Internet






