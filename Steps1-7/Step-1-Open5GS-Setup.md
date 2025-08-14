# 📡 Step 1 – Open5GS Setup

This step covers the installation and configuration **Open5GS** for a private 5G setup.

---

## 1️⃣ Installing MongoDB
```bash
sudo apt update
sudo apt install gnupg
curl -fsSL https://pgp.mongodb.com/server-6.0.asc | sudo gpg -o /usr/share/keyrings/mongodb-server-6.0.gpg --dearmor

echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

sudo apt update
sudo apt install -y mongodb-org
sudo systemctl start mongod
sudo systemctl enable mongod
```


## 2️⃣ Installing Open5GS
```bash
sudo add-apt-repository ppa:open5gs/latest
sudo apt update
sudo apt install open5gs
```



## 3️⃣ Installing Node.js (for WebUI)
```bash
sudo apt update
sudo apt install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg

# Create deb repository
NODE_MAJOR=20
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list

# Install Node.js
sudo apt update
sudo apt install nodejs -y
```


## 4️⃣ Installing Open5GS WebUI
```bash
curl -fsSL https://open5gs.org/open5gs/assets/webui/install | sudo -E bash -
```

## Installation and setup is completed Now we have to configure it to be compatible with srsRAN

## ⚙️ Configuration for srsRAN Compatibility
> 💡 TIP
- **Single Node Setup** (srsGNB and Open5gs on the same PC)  
  No change in IP address required — just change the **MCC** and **MNC** code.
- **Multi-Node Setup** (srsGNB and Open5gs on different PCs)  
  Change the IP address where highlighted in image to IP address of Machine (where you have installed the open5Gs) and **MCC** and **MNC** code.


## In case of Multi-Node setup, we need to replace every IP shown in the image with the IP address of the machine (can get using `ip a` command)


### 1.   /etc/open5gs/mme.yaml configuration
```
 gedit /etc/open5gs/mme.yaml
```
Change the MNC and MCC codes accordingly.
(We have used 001 for MNC and 01 for MCC codes respectively)

The values `001` (MNC) and `01` (MCC) are **Mobile Network Codes** and **Mobile Country Codes** — they identify a mobile network operator and country in mobile communication systems (like LTE, 5G).
**MCC (Mobile Country Code)** → `01`  
- Identifies the country of the mobile network.  
- `01` is a **test MCC** defined by the ITU (not tied to any real-world country).  
- Used in lab setups, simulations, or private 5G networks so they don’t conflict with real operator networks.
**MNC (Mobile Network Code)** → `001`  
- Identifies the specific mobile network operator within the country defined by the MCC.  
- `001` is also a **test MNC** used for experimental setups.
📌 **In short:**  
`MCC = 01` + `MNC = 001` → **Test network identifiers**, safe for private LTE/5G setups without interfering with real carriers.



We have used the TAC as 7.
(It should be the same in the srsRAN configuration)

**TAC = 7 in our setup**  
- Here, we’ve chosen `7` arbitrarily for testing purposes.  
- The same TAC value **must match** between the RAN side (srsRAN) and the core network (Open5GS) configuration — otherwise the UE will reject the attach request.  
- In private or test networks, any unused TAC value can be chosen, but in public networks TACs are allocated by the operator.


No change in IP address is required for our setup.
(Do not change any IP addresses)



### 2.   /etc/open5gs/nrf.yaml configuration
```bash
sudo gedit /etc/open5gs/nrf.yaml
```
- change the MNC and MCC code accordingly as mentioned above, we have used 001 and 01 for MNC and MCC codes respectively.
- We have used the TAC as 7.

![image](https://github.com/user-attachments/assets/5f3f02d0-e10e-4a57-bdb9-5e1dbb0233e8)


### 3.   /etc/open5gs/amf.yaml configuration
```bash
 sudo gedit /etc/open5gs/amf.yaml
```
- change the MNC and MCC code accordingly as mentioned above, we have used 001 and 01 for MNC and MCC codes respectively.
- We have used the TAC as 7.
- **Change the highlighted IP with the IP address of system in case of multinode setup**
![image](https://github.com/user-attachments/assets/f47a4189-0913-4395-b556-40ef4e1b5fbf)


### 4.   /etc/open5gs/upf.yaml configuration
```bash
 sudo gedit /etc/open5gs/upf.yaml
```
- change the MNC and MCC code accordingly as mentioned above, we have used 001 and 01 for MNC and MCC codes respectively.
- We have used the TAC as 7.
- **Change the highlighted IP with the IP address of system in case of multinode setup**
![image](https://github.com/user-attachments/assets/89259622-bea6-48ef-abe3-b20f096d9602)



## Enabling NAT configuration in the machine to access Internet

- Enables IP forwarding in the Linux kernel (lets your machine forward packets between network interfaces, acting like a router).
- Sets up NAT masquerading on interface eth0, so outgoing packets appear as if they come from your machine’s IP (needed for internet sharing).
- Stops UFW (Uncomplicated Firewall) service to avoid firewall rules blocking your forwarding/NAT.
- Adds a rule to allow all forwarded packets through the system.
> - 📌 In short: These commands turn your PC into a simple router by enabling packet forwarding, setting up NAT, and opening firewall rules.

```bash
sudo sysctl -w net.ipv4.ip_forward=1
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo systemctl stop ufw
sudo iptables -I FORWARD 1 -j ACCEPT
```


## Restart all 5g services
```bash
sudo systemctl restart open5gs-*
```

# Now we have completed the core side of the 5g setup 👍 🎉🎉.

