# DNS Setup Guide

This guide records the DNS workflow used for the lab. Replace the documentation-only examples with sanitized values that match your own environment.

## Example Addressing

| Item | Documentation example |
| --- | --- |
| DNS server | `192.0.2.10` |
| Client | `192.0.2.20` |
| Lab domain | `corp.example.test` |
| Example host | `server.corp.example.test` |
| Example alias | `www.corp.example.test` |

The `192.0.2.0/24` network is used here only for documentation.

## 1. Prepare the Windows Server

1. Open the network adapter settings.
2. Assign the server a static IPv4 address.
3. Configure the subnet mask and default gateway for the lab network.
4. Set the preferred DNS server to the Windows Server address.
5. Confirm the configuration:

```powershell
ipconfig /all
```

## 2. Install DNS Server

1. Open **Server Manager**.
2. Select **Manage > Add Roles and Features**.
3. Choose **Role-based or feature-based installation**.
4. Select the correct server.
5. Enable **DNS Server**.
6. Accept the required features and complete the installation.
7. Open **Tools > DNS**.

## 3. Create a Forward Lookup Zone

1. Expand the server in DNS Manager.
2. Right-click **Forward Lookup Zones** and select **New Zone**.
3. Select **Primary zone**.
4. Enter the sanitized lab domain name.
5. Select the appropriate dynamic-update option for the environment.
6. Complete the wizard.

## 4. Create an A Record

1. Open the new forward lookup zone.
2. Select **New Host A or AAAA**.
3. Enter the host label and IPv4 address.
4. Select **Create associated pointer PTR record** if the reverse zone already exists.
5. Add the host.

## 5. Create a Reverse Lookup Zone

1. Right-click **Reverse Lookup Zones** and select **New Zone**.
2. Select **Primary zone** and **IPv4 Reverse Lookup Zone**.
3. Enter the network ID for the sanitized lab network.
4. Select the appropriate dynamic-update option.
5. Complete the wizard.

If the A record was created before the reverse zone, create its PTR record manually or recreate the host record with the pointer option enabled.

## 6. Create a CNAME Alias

1. Open the forward lookup zone.
2. Select **New Alias CNAME**.
3. Enter the alias name, such as `www`.
4. Browse to and select the target host record.
5. Save the alias.

## 7. Configure the Client

1. Open the client network adapter settings.
2. Set the preferred DNS server to the Windows Server address.
3. Clear cached DNS information:

```powershell
ipconfig /flushdns
```

## 8. Test Forward Resolution

```powershell
nslookup server.corp.example.test
ping server.corp.example.test
```

The returned address should match the sanitized address assigned to the host record.

## 9. Test Reverse Resolution

```powershell
nslookup 192.0.2.10
```

The returned name should match the fully qualified domain name configured in the PTR record.

## 10. Test the Alias

```powershell
nslookup www.corp.example.test
ping www.corp.example.test
```

The alias should resolve through its target host.

## Troubleshooting

### The client cannot resolve names

- Confirm that the client points to the Windows Server for DNS.
- Confirm that the DNS Server service is running.
- Check Windows Firewall rules for DNS traffic.
- Clear the client DNS cache and retry.

### Forward lookup works but reverse lookup fails

- Confirm that the reverse lookup zone uses the correct network ID.
- Confirm that a PTR record exists for the host.
- Check that the PTR record points to the correct fully qualified domain name.

### The alias does not resolve

- Confirm that the CNAME target exists as an A record.
- Confirm that the alias is created in the correct forward lookup zone.
- Check for typing differences in the hostname or domain.

## Publication Checklist

- Use only screenshots captured from your own lab.
- Mask real usernames, passwords, public addresses and identifying information.
- Use consistent sanitized hostnames and addresses throughout the documentation.
- Confirm that no screenshot belongs to another student or university submission.

