## Active Directory Lab Environment

This GitHub repo contains the code and documentation for setting up a complete Active Directory (AD) lab using VirtualBox, Windows Server 2019, and Windows 10. The goal is to simulate a basic enterprise environment for learning and testing AD, DHCP, DNS, and NAT configurations.

---

## Requirements

- Oracle VirtualBox
- Windows 10 ISO
- Windows Server 2022 ISO
- Minimum 16GB RAM (32GB Recommended)
- 100GB+ Storage

---

## Lab Setup Overview

### Virtual Machines

| VM Name       | OS                  | Purpose                        | Network Adapter             |
|--------------|---------------------|--------------------------------|-----------------------------|
| `DC-Server`  | Windows Server 2022 | AD, DNS, DHCP, NAT, RAS             | Adapter 1: NAT, Adapter 2: Internal |
| `Win10-Client` | Windows 10         | Domain Joined Client           | Adapter 1: Internal         |

---

## Instructions

### 1. Install Oracle VirtualBox

[Download and install](https://www.virtualbox.org/)

---

### 2. Download ISOs

- Windows 10 ISO: [Download](https://www.microsoft.com/en-us/software-download/windows10)
- Windows Server 2022 ISO: [Download](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022)

---

### 3. Create Virtual Machines

#### Domain Controller VM (DC)

- Set up two network adapters:
  - Adapter 1: NAT (for internet)
  - Adapter 2: Internal Network (`InternalNet`)
- Install Windows Server 2022
- Rename to `DC`

#### Client VM (Win10-Client)

- One network adapter:
  - Adapter 1: Internal Network (`InternalNet`)
- Install Windows 10
- Rename to `Win10-Client`

---

## Configure Domain Controller

### Install Active Directory Domain Services

```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```

### Promote Server to Domain Controller

```powershell
Install-ADDSForest -DomainName "mydomain.com" -DomainNetbiosName "CORP"
```

- After reboot, FQDN: `mydomain.com`

---

## Configure RAS/NAT

- Add Remote Access role via Server Manager
- Use Routing and Remote Access (RRAS) to enable NAT on the external adapter (NAT)
- Internal adapter IP: `172.16.0.1`

---

## Set Up DHCP Server

### Install DHCP Role

```powershell
Install-WindowsFeature DHCP -IncludeManagementTools
```

### Add DHCP Scope

``DHCP Scope
Add-DhcpServerv4Scope -Name "InternalScope" -StartRange 172.16.0.100 -EndRange 172.16.0.200 -SubnetMask 255.255.255.0
```

- Authorize DHCP and start the service

---

## PowerShell Script: Create 1000 Users

Create a script file `Create-Users.ps1`:

## Edit these varibale for your own use case

```powershell
$password_for_users = "Password1"
$USER_FIRST_LAST_LIST = Get-content .\name.text #this is to used if you have name tored in text file
##---------------------------------------
$password = ConverTo-securesstring $PASSWORD_USERS -AsPlainText -Force
New-ADOrganizationalUnit -name_USERS -ProtectedFromAccidentalDeletion $false

foreach ($n in $User_FIRST_LAST_LIST) {
  $First = $n.split (" ") [0] . ToLower ()
  $Last = 4n.split (" ") [1] . ToLowr ()
  $username = "$($first.substring(0,1)$($last)".ToLowwer()
  writ-Host "Creating user: $($username)" -backroundcolor Black -Foregroundcolor Cyan #you can set any color you want here

  new-AdUser -AccountPassowrd $Password `
             -givenname $first `
             -surname $last `
             -DisplayName $usrname `
             -name $username `
             -EmplayeeID $username `
             -PasswordnverExpires $true #can be change 
             =path "ou=_USERS,$(([ADSI] `"") .distingushedName)" `
}
```

> Make sure the OU `Users` exists before running.

---

## Configure Client VM

- Set NIC to Internal Network
- Boot and confirm IP is received from DHCP
- Join the domain:
  - Domain: `mydomain.com`
  - Use domain admin credentials

---

## Final Verification

Ensure that the client:

- Receives a valid IP from DHCP
- Can ping `DC`
- Has internet access through NAT
- Can log in with a domain account

---

## Repository Structure

```bash
ActiveDirectory-Lab/
├── Create-Users.ps1
├── README.md
└── Screenshots/         # Optional screenshots
    ├── VM_Settings.png
    ├── DHCP_Config.png
    └── AD_Users.png
```

---

## License

This project is for educational use. Feel free to fork and modify!


