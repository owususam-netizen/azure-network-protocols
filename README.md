# azure-network-protocols
Hands-on Azure networking lab exploring Wireshark, network protocols, virtual machines, and firewall security through practical traffic analysis and troubleshooting.


<p align="center">
  <img src="https://i.imgur.com/Ua7udoS.png" alt="Network Traffic Analysis" width="600"/>
</p>

<h1 align="center">🌐 Network Traffic Analysis & Security with Microsoft Azure</h1>

<p align="center">
  <strong>Exploring How Computers Communicate and How Firewalls Protect Them</strong>
</p>

---

## 📖 Project Overview

Have you ever wondered how computers talk to each other over a network?

In this project, I built two virtual computers in **Microsoft Azure** and used a tool called **Wireshark** to watch how they communicate.

Think of Wireshark as a security camera for network traffic. It lets us see the messages computers send back and forth.

I also used **Network Security Groups (NSGs)**, which work like security guards. They decide which network traffic is allowed through and which traffic gets blocked.

### 🎯 What I Wanted to Learn

- How computers communicate over a network.
- How to capture and inspect network traffic.
- How different network protocols work.
- How to allow or block traffic using firewall rules.
- How to troubleshoot basic network connection problems.

---

## 🛠️ Tools and Technologies

| Tool | What It Does |
|------|--------------|
| **Microsoft Azure** | Creates virtual computers in the cloud |
| **Wireshark** | Captures and displays network traffic |
| **Network Security Groups** | Controls which traffic can enter or leave a network |
| **Remote Desktop (RDP)** | Allows remote access to a Windows computer |
| **Command Prompt / PowerShell** | Runs commands to test network connections |
| **SSH** | Provides a secure way to access another computer |

### 💻 Operating Systems

- **Windows 10 (21H2)** — Main computer used to inspect traffic.
- **Ubuntu Server 20.04** — Linux computer used for network testing.

---

## 🧠 Understanding Network Protocols

Before starting, let's understand the different types of network traffic.

A **network protocol** is simply a set of rules computers follow when communicating.

| Protocol | Simple Explanation |
|----------|--------------------|
| **ICMP** | Checks whether another computer can respond |
| **SSH** | Lets you securely control another computer |
| **DHCP** | Helps computers obtain IP addresses |
| **DNS** | Translates website names into IP addresses |
| **RDP** | Lets you view and control another computer remotely |

Think of it this way:

- **ICMP:** "Hello! Can you hear me?"
- **SSH:** "Let me securely control that computer."
- **DHCP:** "I need an IP address so I can join the network."
- **DNS:** "What's the IP address for google.com?"
- **RDP:** "Let me see and control that computer's screen."

---

# 🚀 Part 1: Building My Virtual Network

The first step was creating two virtual machines (VMs) in Microsoft Azure.

A **virtual machine** is a computer that runs inside another computer or cloud environment.

### Steps I Followed

1. Logged into the Microsoft Azure Portal.
2. Created a **Resource Group** to organize my project.
3. Created a Windows 10 virtual machine.
4. Created an Ubuntu Linux virtual machine.
5. Connected both machines to the same **Virtual Network (VNet)**.
6. Used Azure Network Watcher to examine the network setup.

### Why This Matters

Both computers need a network connection before they can communicate.

Think of a Virtual Network like a neighborhood. The virtual machines are houses, and the network allows messages to travel between them.

**✅ Result:** Two virtual machines were created inside the same Azure network.

---

# 📡 Part 2: Watching ICMP Traffic (Ping)

Next, I tested whether my Windows computer could communicate with my Linux computer.

I used **ICMP**, which is commonly used by the `ping` command.

### Steps I Followed

1. Connected to the Windows VM using Remote Desktop.
2. Installed and opened Wireshark.
3. Entered the following Wireshark display filter:

```text
icmp
```

4. Found the private IP address of the Ubuntu VM.
5. Opened Command Prompt on Windows.
6. Sent a ping to the Ubuntu VM:

```powershell
ping <Ubuntu_Private_IP>
```

7. Watched the ICMP request and reply packets in Wireshark.
8. Tested connectivity to a public website:

```powershell
ping www.google.com
```

### What I Learned

When a computer sends a ping, it is basically asking:

**Computer A:** "Hey, are you there?"

**Computer B:** "Yes, I'm here!"

Wireshark allows us to see these messages traveling across the network.

**✅ Result:** I observed ICMP requests and replies between the virtual machines.

---

## 🔒 Testing Network Security Group Rules

Now it was time to see what happens when network traffic gets blocked.

An **NSG (Network Security Group)** works like a security guard for an Azure network.

It checks network traffic against security rules to decide what is allowed and what is blocked.

### Steps I Followed

1. Started a continuous ping from Windows to Ubuntu:

```powershell
ping -t <Ubuntu_Private_IP>
```

2. Opened the Azure Portal.
3. Located the Network Security Group protecting the Ubuntu VM.
4. Added an inbound rule to block ICMP traffic from the Windows VM.
5. Returned to Wireshark and Command Prompt.
6. Observed that ping requests stopped receiving replies.
7. Removed or disabled the blocking rule.
8. Confirmed that ping replies started working again.
9. Stopped the continuous ping using `Ctrl + C`.

### What Happened?

**Before blocking ICMP:**

```text
Windows VM  ---- Ping Request ---->  Ubuntu VM
Windows VM  <--- Ping Reply ------  Ubuntu VM

Connection Successful!
```

**After blocking ICMP:**

```text
Windows VM  ---- Ping Request ---->  [BLOCKED]

Request timed out.
```

**After allowing ICMP again:**

```text
Windows VM  ---- Ping Request ---->  Ubuntu VM
Windows VM  <--- Ping Reply ------  Ubuntu VM

Connection Restored!
```

### What I Learned

Firewall rules can control whether computers are allowed to communicate.

Even if two computers are connected to the same network, security rules can prevent certain types of traffic.

**✅ Result:** I successfully tested how blocking and allowing ICMP traffic affects network connectivity.

---

# 🔐 Part 3: Inspecting SSH Traffic

Next, I explored **SSH (Secure Shell)**.

SSH allows administrators to securely connect to another computer and run commands.

### Steps I Followed

1. Opened Wireshark on the Windows VM.
2. Applied the following display filter:

```text
tcp.port == 22
```

3. Opened PowerShell.
4. Connected to the Ubuntu VM using SSH:

```powershell
ssh username@<Ubuntu_Private_IP>
```

5. Entered the required login credentials.
6. Ran basic commands inside the Linux machine.
7. Watched the SSH network packets in Wireshark.
8. Closed the SSH session using:

```bash
exit
```

### What I Learned

SSH creates an encrypted connection between computers.

This means someone watching the network can see that SSH traffic exists, but cannot easily read the commands or passwords being transmitted.

**✅ Result:** I established an SSH connection and observed encrypted traffic in Wireshark.

---

# 🏠 Part 4: Understanding DHCP Traffic

Every computer on a network needs an IP address.

**DHCP (Dynamic Host Configuration Protocol)** helps devices automatically obtain IP configuration information.

Think of DHCP like a hotel receptionist assigning a room number to a guest.

### Steps I Followed

1. Opened Wireshark.
2. Applied the following filter:

```text
dhcp
```

3. Opened Command Prompt.
4. Attempted to renew the Windows VM's IP configuration:

```powershell
ipconfig /renew
```

5. Checked Wireshark for DHCP-related traffic.

### What I Learned

DHCP allows computers to obtain network settings without requiring administrators to manually configure every device.

A common DHCP process is called **DORA**:

1. **Discover:** The computer looks for a DHCP server.
2. **Offer:** The server offers an IP address.
3. **Request:** The computer requests the offered address.
4. **Acknowledge:** The server confirms the assignment.

**✅ Result:** I explored how DHCP supports automatic IP address configuration.

> **Note:** Azure manages DHCP for its virtual networks. A renewal command may not show the complete DORA process, and an existing address may be renewed without changing.

---

# 🌍 Part 5: Inspecting DNS Traffic

Next, I explored **DNS (Domain Name System)**.

DNS works like the internet's phone book.

Instead of remembering a website's IP address, we can simply type its name.

For example:

```text
google.com → IP Address
```

### Steps I Followed

1. Opened Wireshark.
2. Applied the DNS display filter:

```text
dns
```

3. Opened Command Prompt.
4. Looked up the IP address of Google:

```powershell
nslookup google.com
```

5. Repeated the process with another website:

```powershell
nslookup disney.com
```

6. Examined the DNS requests and responses in Wireshark.

### What I Learned

DNS translates human-friendly website names into IP addresses that computers use to communicate.

Without DNS, accessing websites would be much harder because we would need to remember their IP addresses.

**✅ Result:** I captured and inspected DNS lookup traffic.

---

# 🖥️ Part 6: Inspecting Remote Desktop (RDP) Traffic

Finally, I explored **Remote Desktop Protocol (RDP)**.

RDP allows users and IT administrators to connect to and control Windows computers remotely.

### Steps I Followed

1. Connected to the Windows VM using Remote Desktop.
2. Opened Wireshark inside the Windows VM.
3. Applied the following filter:

```text
tcp.port == 3389
```

4. Observed the RDP traffic between my local computer and the Windows VM.
5. Moved the mouse and interacted with the remote desktop.
6. Continued monitoring the packets in Wireshark.

### What I Learned

RDP sends information between computers to maintain a remote session.

This can include screen updates, keyboard input, mouse movements, and other session data.

Even when I wasn't actively clicking anything, I could still observe network activity associated with the connection.

**✅ Result:** I observed RDP network traffic during an active remote desktop session.

> **Note:** RDP can also use UDP port 3389. The TCP filter above displays only TCP traffic.

---

# 📊 Project Summary

Throughout this lab, I explored several network protocols and learned how security rules affect communication between computers.

| Protocol | What I Tested | Main Lesson |
|----------|---------------|-------------|
| **ICMP** | Ping between virtual machines | How to test network connectivity |
| **SSH** | Remote connection to Linux | How secure remote administration works |
| **DHCP** | IP address renewal | How devices receive network settings |
| **DNS** | Website name lookups | How domain names become IP addresses |
| **RDP** | Remote Windows connection | How remote desktop communication works |
| **NSG** | Blocking and allowing ICMP | How security rules control network traffic |

---

# 🏆 Skills Demonstrated

This project helped me develop hands-on experience with:

- Microsoft Azure Virtual Machines
- Azure Virtual Networks and Network Security Groups
- Wireshark packet analysis
- Network connectivity testing
- ICMP, SSH, DHCP, DNS, and RDP
- Windows and Linux networking
- Command Prompt and PowerShell
- Firewall configuration
- Remote Desktop administration
- Basic network security and troubleshooting

---

# 🎯 Final Thoughts

This project helped me better understand what happens behind the scenes when computers communicate.

By creating two virtual machines in Microsoft Azure, I was able to test different network protocols, inspect traffic using Wireshark, and see how firewall rules can allow or block communication.

One of the most important lessons I learned was that **being connected to a network doesn't always mean two computers can communicate**. Security rules, firewall settings, and network configurations all play a role.

Overall, this lab strengthened my understanding of networking, cloud infrastructure, troubleshooting, and security.

These are important skills for **IT Support, Help Desk, Network Technician, and System Administrator** roles.

---

## 🔮 Future Improvements

As I continue developing my IT skills, I plan to expand this project by:

- Capturing and analyzing HTTP and HTTPS traffic.
- Exploring additional firewall rules.
- Troubleshooting common network connectivity issues.
- Learning more about TCP and UDP communication.
- Practicing advanced Azure networking configurations.
