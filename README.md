# DNS-Only Clash Configuration (with TUN & Global Modes)

[![GitHub stars](https://img.shields.io/github/stars/xoxxel/dns-only-clash.svg?style=social)](https://github.com/xoxxel/dns-only-clash/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/xoxxel/dns-only-clash.svg?style=social)](https://github.com/xoxxel/dns-only-clash/network/members)

This project provides a fully working `dns-only` configuration for [Clash](https://www.clashforwindows.net), including:

* DNS-over-HTTPS support
* TUN (transparent proxy) mode
* System-wide proxy for bypassing censorship
* Tested configuration on **Windows**

---

## ✅ Features

* 🚀 DNS hijacking with `fake-ip` mode
* 🔐 HTTPS DNS via Cloudflare, Google, and Quad9
* 🌍 TUN mode for full device routing
* 🧩 Easily testable with `nslookup`, `curl`, or browser
* 🛡️ Selective domain routing via rules

---

## 📦 Installation

### 1. Download Clash

Choose one of the following:

* [Clash for Windows (official)](https://www.clashforwindows.net) --- Recommended 
* [Clash Verge (GUI + TUN support)](https://github.com/clash-verge-rev/clash-verge-rev)
* [Clash Meta (CLI)](https://github.com/MetaCubeX/mihomo)

### 2. Get the Configuration

Clone this repo or download the files:

```bash
git clone https://github.com/xoxxel/dns-only-clash.git
```

Place the file `dns-only.yaml` or `config.yaml` inside Clash's config folder.

### 3. Download & Place `wintun.dll`

Required for TUN mode on Windows.

* Download `wintun.dll` from:
  [https://www.wintun.net/](https://www.wintun.net/)

Place `wintun.dll` beside your `clash.exe` file or in the `resources/static/files` folder of Clash GUI builds.

---

## 🧪 Testing the Setup

### A. Basic DNS test

```bash
nslookup -port=5454 google.com 127.0.0.1
```

> If it returns an IP address, DNS is working.

### B. Proxy test

```powershell
Invoke-WebRequest -Uri "https://www.google.com" -Proxy "http://127.0.0.1:7890"
```

> If you get StatusCode 200, Clash is working.

### C. Test with browser

* Firefox → `about:config`
* Set:

  * `network.trr.mode` → `3`
  * `network.trr.uri` → `https://127.0.0.1:5454/dns-query`

Test browsing: [https://dnsleaktest.com](https://dnsleaktest.com)

---

## 🔧 Configuration Overview

### `dns-only.yaml` example

```yaml
dns:
  enable: true
  listen: 127.0.0.1:5454
  enhanced-mode: fake-ip
  nameserver:
    - https://1.1.1.1/dns-query
    - https://8.8.8.8/dns-query
  fallback:
    - https://1.0.0.1/dns-query
    - https://9.9.9.9/dns-query
```

### TUN mode

```yaml
tun:
  enable: true
  stack: system
  dns-hijack:
    - any:53
```

Make sure `mixed-port: 7890` is also defined.

---

## 🖼️ Mode Diagrams

### 🌐 Global Proxy Mode

```
+-----------+      +--------+       +-----------------+
|  Browser  +----->+ Clash  +-----> | Proxy or Direct |
+-----------+      +--------+       +-----------------+
         |           |
         | DNS       | DNS Proxy (DoH)
         v           v
    127.0.0.1:53  DNS Providers
```

### 🔄 TUN Mode (System-Wide)

```
+-----------+   TUN Adapter   +--------+       +-----------------+
|  Windows  +--------------->+ Clash  +-----> | Proxy or Direct |
+-----------+                +--------+       +-----------------+
           DNS Hijack: any:53
```

---

## ✅ Best Practices & Notes

* Always run Clash with **Administrator privileges** when using TUN
* Avoid using port `53` if it’s occupied; use `5454` instead
* Use `route print` or `Get-NetAdapter` to inspect TUN status
* Place `wintun.dll` properly to avoid silent TUN failure

---

## 🧩 Advanced Testing

* `curl.exe --proxy http://127.0.0.1:7890 https://google.com`
* `ping 8.8.8.8` (to verify raw connectivity)
* `Get-NetAdapter` (look for TUN or Wintun interfaces)
* Firefox `about:networking#dns` to inspect active DNS

---
## ❗ Common Problems & Fixes
   💥 TUN Mode Causes Internet Disconnect?
   
   Run these commands to reset Windows networking stack:

* `netsh winsock reset`
* `netsh int ip reset`
* `ipconfig /flushdns`

  Then restart Clash with Admin rights.

---

## 📣 Contributing

Feel free to open an issue or PR if:

* You tested this on Linux/macOS
* You improved routing logic
* You want to support custom proxy groups

---

## 📎 Repository Link

Find all configuration files, updates, and instructions here:
🔗 **[https://github.com/xoxxel/dns-only-clash](https://github.com/xoxxel/dns-only-clash)**

---

## 📜 License

MIT © xoxxel
