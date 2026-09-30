# 🛡️ AdGuard DNS Complete Setup Guide

A complete, practical tutorial for configuring **AdGuard DNS** across all 3 official modes (Default, Family Protection, and Non-filtering) on home routers, mobile devices, operating systems, and web browsers — including solutions for IPv6 DNS leaks and browser DNS-over-HTTPS bypasses.

---

## 📑 Table of Contents
1. [Overview of AdGuard DNS Modes](#1-overview-of-adguard-dns-modes)
2. [DNS Server Addresses & Protocols](#2-dns-server-addresses--protocols)
   - [Option 1: Default (Ad-Blocking & Tracking Protection)](#option-1-default-ad-blocking--privacy)
   - [Option 2: Family Protection (Child-Safe & 18+ Filter)](#option-2-family-protection-child-safe--18-content-filter)
   - [Option 3: Non-Filtering (Unfiltered & Fast)](#option-3-non-filtering-unfiltered--pure-resolver)
3. [Router Configuration Guide](#3-router-configuration-guide)
   - [Step 1: IPv4 LAN DHCP Setup](#step-1-ipv4-lan-dhcp-setup)
   - [Step 2: IPv6 Configuration (Critical to Prevent DNS Leaks)](#step-2-ipv6-configuration-preventing-dns-leaks)
   - [Special: ISP ONT / Realtek Router Hidden Settings](#special-isp-provided-routers-ont--realtek-firmware)
4. [Device & Client Setup](#4-device--client-setup)
   - [Android (Private DNS)](#android-private-dns)
   - [iOS & iPadOS (iPhone / iPad)](#ios--ipados-iphone--ipad)
   - [Windows 11 / 10](#windows-11--10)
   - [macOS](#macos)
   - [Linux (systemd-resolved / NetworkManager)](#linux-systemd-resolved--networkmanager)
5. [Browser Troubleshooting: Secure DNS / DoH Bypass](#5-browser-troubleshooting-secure-dns--doh-bypass)
6. [Verification & Testing](#6-verification--testing)

---

## 1. Overview of AdGuard DNS Modes

AdGuard provides 3 public DNS configurations designed for different needs:

| Feature | Option 1: Default | Option 2: Family Protection | Option 3: Non-Filtering |
| :--- | :---: | :---: | :---: |
| **Blocks Ads & Popups** | ✅ Yes | ✅ Yes | ❌ No |
| **Blocks Trackers & Analytics** | ✅ Yes | ✅ Yes | ❌ No |
| **Blocks Phishing & Malware** | ✅ Yes | ✅ Yes | ❌ No |
| **Blocks 18+ / Adult Content** | ❌ No | ✅ **Yes** | ❌ No |
| **Enforces SafeSearch (Google/Bing/etc)** | ❌ No | ✅ **Yes** | ❌ No |
| **Raw Unfiltered Resolution** | ❌ No | ❌ No | ✅ **Yes** |

---

## 2. DNS Server Addresses & Protocols

### Option 1: Default (Ad-Blocking & Privacy)
*Best for everyday general use, blocking ads, popups, and user-tracking telemetry.*

* **IPv4 Addresses:**
  * Primary: `94.140.14.14`
  * Secondary: `94.140.15.15`
* **IPv6 Addresses:**
  * Primary: `2a10:50c0::ad1:ff`
  * Secondary: `2a10:50c0::ad2:ff`
* **DNS-over-HTTPS (DoH):**
  * `https://dns.adguard-dns.com/dns-query`
* **DNS-over-TLS (DoT) / Android Private DNS:**
  * `dns.adguard-dns.com`
* **DNSCrypt Server Name:**
  * `adguard-dns`

---

### Option 2: Family Protection (Child-Safe & 18+ Content Filter)
*Best for households with kids. Automatically blocks pornographic/adult websites, forces SafeSearch on all major search engines, and blocks ads/trackers.*

* **IPv4 Addresses:**
  * Primary: `94.140.14.15`
  * Secondary: `94.140.15.16`
* **IPv6 Addresses:**
  * Primary: `2a10:50c0::bad1:ff`
  * Secondary: `2a10:50c0::bad2:ff`
* **DNS-over-HTTPS (DoH):**
  * `https://dns-family.adguard.com/dns-query`
* **DNS-over-TLS (DoT) / Android Private DNS:**
  * `family.adguard-dns.com`
* **DNSCrypt Server Name:**
  * `adguard-dns-family`

---

### Option 3: Non-Filtering (Unfiltered & Pure Resolver)
*Best if you do not want any filtering, blocking, or modification of DNS responses, but want a secure, privacy-respecting DNS resolver.*

* **IPv4 Addresses:**
  * Primary: `94.140.14.140`
  * Secondary: `94.140.14.141`
* **IPv6 Addresses:**
  * Primary: `2a10:50c0::1:ff`
  * Secondary: `2a10:50c0::2:ff`
* **DNS-over-HTTPS (DoH):**
  * `https://dns-unfiltered.adguard.com/dns-query`
* **DNS-over-TLS (DoT) / Android Private DNS:**
  * `unfiltered.adguard-dns.com`
* **DNSCrypt Server Name:**
  * `adguard-dns-unfiltered`

---

## 3. Router Configuration Guide

Configuring AdGuard DNS on your router protects every device connected to your home network (phones, tablets, Smart TVs, IoT devices, laptops) without installing software on each device.

### Step 1: IPv4 LAN DHCP Setup
1. Open your browser and navigate to your router gateway IP (typically `192.168.1.1` or `192.168.0.1`).
2. Log in with your admin credentials.
3. Locate **Network** $\rightarrow$ **LAN** (or **DHCP Server**).
4. Change **DNS Mode** from *Auto / From ISP* to **Manual / Custom**.
5. Enter the AdGuard IP addresses for your preferred mode:
   * **Primary DNS:** `94.140.14.15` (Family) or `94.140.14.14` (Default)
   * **Secondary DNS:** `94.140.15.16` (Family) or `94.140.15.15` (Default)
6. Save and apply settings.

---

### Step 2: IPv6 Configuration (Preventing DNS Leaks)
> ⚠️ **CRITICAL STEP**: If your ISP provides IPv6 (common with modern fiber connections), devices will prioritize IPv6 DNS queries over IPv4. If you leave IPv6 DNS untouched, all DNS queries will bypass AdGuard through your ISP!

1. In your router settings, navigate to **IPv6** $\rightarrow$ **LAN Configuration** (or **RADVD / DHCPv6**).
2. Change **IPv6 DNS Mode** to **Manual / Static**.
3. Set the Primary & Secondary IPv6 DNS:
   * **Family Protection:** `2a10:50c0::bad1:ff` and `2a10:50c0::bad2:ff`
   * **Default Ad-Blocking:** `2a10:50c0::ad1:ff` and `2a10:50c0::ad2:ff`
4. If your router has a **Router Advertisement (RADVD)** or **RDNSS** section, ensure RDNSS 1 and RDNSS 2 are configured with these IPv6 addresses.
5. Save settings and reboot the router.

---

### Special: ISP-Provided Routers (ONT / Realtek Firmware)
Many ISP routers (e.g. Excitel, Syrotech, Netlink, C-Data, Huawei XPON ONUs) hide the DNS configuration fields using CSS (`display: none`) in their firmware template.
* If the fields are hidden in the web interface, inspect the HTML using Chrome/Edge DevTools (F12) to reveal the hidden input fields (`optID`, `dhcpdns`, `dns1`, `dns2`).
* Or configure them via the router API / management interface.

---

## 4. Device & Client Setup

### Android (Private DNS)
Android 9+ natively supports encrypted DNS-over-TLS (DoT):
1. Go to **Settings** $\rightarrow$ **Network & internet** $\rightarrow$ **Private DNS**.
2. Select **Private DNS provider hostname**.
3. Enter one of the following:
   * Family: `family.adguard-dns.com`
   * Default: `dns.adguard-dns.com`
   * Non-filtering: `unfiltered.adguard-dns.com`
4. Tap **Save**.

### iOS & iPadOS (iPhone / iPad)
* **Wi-Fi DNS:** Settings $\rightarrow$ Wi-Fi $\rightarrow$ Tap `(i)` next to your network $\rightarrow$ Configure DNS $\rightarrow$ Manual $\rightarrow$ Add AdGuard IPv4 & IPv6 IPs.
* **Encrypted DoH/DoT System Profile:** Visit [AdGuard DNS Knowledgebase](https://adguard-dns.io/) to install a signed `.mobileconfig` DNS profile.
* *Note:* Disable *iCloud Private Relay* if you want your router\s local DNS rules to apply.

### Windows 11 / 10
1. Open **Settings** $\rightarrow$ **Network & internet** $\rightarrow$ **Wi-Fi** (or **Ethernet**).
2. Click **Hardware properties** $\rightarrow$ **DNS server assignment** $\rightarrow$ **Edit**.
3. Select **Manual**, toggle **IPv4** on, and enter:
   * Preferred DNS: `94.140.14.15`
   * Alternate DNS: `94.140.15.16`
4. Toggle **IPv6** on and enter:
   * Preferred DNS: `2a10:50c0::bad1:ff`
   * Alternate DNS: `2a10:50c0::bad2:ff`
5. Click **Save**.

### Linux (systemd-resolved / NetworkManager)
Configure `/etc/systemd/resolved.conf`:
```ini
[Resolve]
DNS=94.140.14.15#family.adguard-dns.com 2a10:50c0::bad1:ff#family.adguard-dns.com
FallbackDNS=94.140.15.16#family.adguard-dns.com 2a10:50c0::bad2:ff#family.adguard-dns.com
DNSOverTLS=yes
```
Restart the service:
```bash
sudo systemctl restart systemd-resolved
```

---

## 5. Browser Troubleshooting: Secure DNS / DoH Bypass

Modern web browsers (Brave, Google Chrome, Mozilla Firefox, Microsoft Edge) often have a feature called **Secure DNS (DNS-over-HTTPS)** enabled by default that bypasses your operating system and router DNS!

If the diagnostic page shows AdGuard is not running despite router configuration:

### Brave Browser
1. Go to `brave://settings/security`.
2. Scroll to **Use secure DNS**.
3. Either:
   * Choose **With your current service provider** (or turn it Off) to inherit router settings.
   * Choose **Custom** and enter: `https://dns-family.adguard.com/dns-query`.

### Google Chrome
1. Go to `chrome://settings/security`.
2. Scroll to **Use secure DNS**.
3. Choose **With your current service provider** or choose **Custom** with AdGuard DoH.

### Mozilla Firefox
1. Go to `about:preferences#privacy`.
2. Scroll down to **DNS over HTTPS**.
3. Choose **Off** (to use router DNS) or choose **Custom** and enter: `https://dns-family.adguard.com/dns-query`.

---

## 6. Verification & Testing

### 1. Web Diagnostic Test
Visit the official AdGuard diagnostics page:
👉 **[https://adguard.com/en/test.html](https://adguard.com/en/test.html)**

You should see:
* 🟢 **AdGuard DNS: Public server: running**
* *"You are using the Family protection AdGuard DNS configuration"* (or Default).

### 2. Command Line (CLI) Tests
* **Test Ad Blocking:**
  ```bash
  dig doubleclick.net +short
  # Returns: 0.0.0.0 (Blocked)
  ```
* **Test Family Safe Filtering:**
  ```bash
  dig pornhub.com +short
  # Returns: 94.140.14.35 (AdGuard Block Landing Page)
  ```

---

## 📄 License
MIT License. Feel free to share and modify.
