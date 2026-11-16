# Firewall Configuration Examples: Palo Alto · Fortinet · Juniper SRX

Reference configurations for the techniques I use in production firewall projects. The real projects are in [network-security-projects](https://github.com/mihirchandra10/network-security-projects). Each lab has a goal, the configuration, and the commands I use to verify it.

> All addresses are lab/documentation values. Keys and passwords are `<PLACEHOLDERS>`.
> The labs for each vendor build on each other: objects such as profile groups and address entries are defined in an earlier lab and reused later. Exact syntax and category IDs vary between OS versions (PAN-OS 10/11, FortiOS 7.x, Junos 21+), so check against your release.

## Lab topology (EVE-NG / GNS3)

```
                 [ Internet / ISP sim ]
                          |
                    untrust 192.0.2.0/29
                          |
          +-------- [ NGFW under test ] --------+
          |               |                     |
   trust 10.10.0.0/24   dmz 172.16.10.0/24   vpn (IPsec / remote access)
   users + AD/DNS       web server           branch / remote users
```

The same topology is used for all three vendors so the results can be compared.

## Labs

### 🔶 Palo Alto Networks (PAN-OS): [`paloalto/`](paloalto/)
| Lab | Topic |
|---|---|
| [01](paloalto/01-app-id-user-id-policy.set) | App-ID and User-ID security policy (least privilege by application and AD group) |
| [02](paloalto/02-threat-prevention-profiles.set) | Threat prevention: AV, anti-spyware with DNS sinkhole, vulnerability protection, WildFire, file blocking |
| [03](paloalto/03-url-filtering-ssl-decryption.set) | URL filtering and SSL forward-proxy decryption with no-decrypt exceptions |
| [04](paloalto/04-zone-protection-dos.set) | Zone protection and DoS protection (flood, reconnaissance, packet-based attacks) |
| [05](paloalto/05-globalprotect-vpn.set) | GlobalProtect remote-access VPN |

### 🔴 Fortinet (FortiOS): [`fortinet/`](fortinet/)
| Lab | Topic |
|---|---|
| [01](fortinet/01-utm-security-profiles.conf) | UTM profiles: IPS, AV, web filter, application control |
| [02](fortinet/02-ssl-deep-inspection.conf) | SSL deep inspection with exemptions |
| [03](fortinet/03-sdwan-dual-wan.conf) | SD-WAN across two ISPs with performance SLA health checks |
| [04](fortinet/04-ssl-vpn-remote-access.conf) | SSL VPN remote access with group-based policy |
| [05](fortinet/05-dos-policy-and-geoip.conf) | DoS policy and geo-IP blocking |

### 🟢 Juniper SRX (Junos): [`juniper-srx/`](juniper-srx/)
| Lab | Topic |
|---|---|
| [01](juniper-srx/01-zones-policies-nat.conf) | Zones, security policies, source/destination/static NAT |
| [02](juniper-srx/02-screens-ids.conf) | Screen options: flood, scan and malformed-packet protection |
| [03](juniper-srx/03-idp-ips.conf) | IDP (intrusion prevention) policy |
| [04](juniper-srx/04-utm-content-security.conf) | UTM / Content Security: antivirus, web filtering, anti-spam |
| [05](juniper-srx/05-appsecure-appfw.conf) | AppSecure: application firewall and AppTrack |

## Vendor feature map

| Capability | Palo Alto | Fortinet | Juniper SRX |
|---|---|---|---|
| App-aware policy | App-ID | Application Control | AppFW / AppID |
| IPS | Vulnerability Protection | IPS sensor | IDP |
| Antivirus | Antivirus profile | AV profile | UTM / Content Security AV |
| URL filtering | URL Filtering | Web Filter | Enhanced Web Filtering |
| SSL inspection | Decryption policy | SSL/SSH inspection | SSL Proxy |
| DoS / flood | Zone & DoS protection | DoS policy | Screens |
| Remote access VPN | GlobalProtect | SSL VPN / IPsec dial-up | Juniper Secure Connect |
| HA | Active/passive, active/active | FGCP | Chassis cluster |

HA and site-to-site VPN configs are in [network-config-templates](https://github.com/mihirchandra10/network-config-templates). The production projects behind these labs are in [network-projects-portfolio](https://github.com/mihirchandra10/network-projects-portfolio).

## Certifications behind this work
Palo Alto ACE · Fortinet NSE 1 & 2 · JNCIA-Junos · CCNA R&S
