# Active Directory and DNS Lab

This repository documents a university Windows Server lab focused on deploying and validating Domain Name System services. The DNS phase is documented first; the Active Directory phase will be added separately when the supporting material is available.

## Project Overview

The lab demonstrates how a Windows Server can provide reliable name resolution for devices inside a controlled network. It covers DNS role installation, forward and reverse lookup zones, common DNS records, and client-side testing.

## Objectives

- Install and configure the DNS Server role on Windows Server.
- Configure forward lookup for hostname-to-IP resolution.
- Configure reverse lookup for IP-to-hostname resolution.
- Create and validate host, pointer, and alias records.
- Test DNS resolution and network connectivity from a client system.
- Document the configuration using sanitized examples and original evidence.

## Environment

| Component | Purpose |
| --- | --- |
| Windows Server | Hosts and manages the DNS service |
| Windows client | Tests hostname and reverse resolution |
| DNS Manager | Creates zones and resource records |
| PowerShell or Command Prompt | Runs connectivity and DNS tests |
| Virtual or physical network | Connects the server and client systems |

## DNS Implementation

### 1. Server preparation

The DNS server is assigned a static IPv4 address before the role is installed. A stable address ensures that clients can consistently reach the DNS service.

### 2. DNS Server role installation

The DNS Server role is installed through Server Manager using **Add Roles and Features**. After installation, DNS Manager is used to configure zones and records.

### 3. Forward lookup zone

A primary forward lookup zone is created for the lab domain. This zone resolves hostnames to IPv4 addresses using `A` records.

### 4. Reverse lookup zone

An IPv4 reverse lookup zone is created for the lab network. This zone resolves IPv4 addresses back to hostnames using `PTR` records.

### 5. DNS records

- **A record:** Maps a hostname to an IPv4 address.
- **PTR record:** Maps an IPv4 address back to a hostname.
- **CNAME record:** Creates an alias that points to an existing host record.

### 6. Validation

The configuration is tested from the server and client using:

```powershell
ipconfig /all
ipconfig /flushdns
nslookup server.corp.example.test
nslookup 192.0.2.10
ping server.corp.example.test
```

The domain and addresses above are documentation-only examples. Real lab identifiers should be removed or masked before screenshots are published.

## Expected Results

- The DNS service starts successfully.
- The client uses the Windows Server address as its preferred DNS server.
- Forward lookup returns the correct test IPv4 address.
- Reverse lookup returns the correct fully qualified domain name.
- The alias resolves to its target host.
- The client can reach the configured host by name.

## Repository Structure

```text
active-directory-dns-lab
├── docs
│   └── dns-setup.md
└── README.md
```

## Evidence To Add

Only original screenshots from the completed lab should be added. Recommended evidence includes:

1. DNS role visible in Server Manager.
2. Forward lookup zone and sanitized `A` record.
3. Reverse lookup zone and sanitized `PTR` record.
4. Sanitized `CNAME` record.
5. Successful forward `nslookup` result.
6. Successful reverse `nslookup` result.
7. Successful hostname connectivity test.

Do not publish passwords, private keys, public IP addresses, student records, unrelated university submissions, or another student’s work.

## Security Considerations

- Use secure dynamic updates when DNS is integrated with Active Directory.
- Restrict zone transfers to authorized DNS servers.
- Apply least privilege to DNS administration.
- Keep Windows Server patched and review DNS event logs.
- Avoid exposing internal DNS zones directly to the public internet.
- Sanitize hostnames and IP addresses before publishing documentation.

## Skills Demonstrated

- Windows Server administration
- DNS installation and configuration
- Forward and reverse name resolution
- DNS record management
- Command-line troubleshooting
- Network testing and technical documentation

## Future Update

The Active Directory Domain Services configuration will be added as the next phase of this repository.

