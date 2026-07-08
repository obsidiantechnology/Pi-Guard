#  Family-oriented Home Network Management: Using Pi-hole as a content filter

This project utilises Pi-hole; a network-wide DNS sinkhole and makes use of its ability to configure upstream DNS to protect family members and children from  adult content, malicious websites, and invasive tracking before it ever reaches a device.

---

## Project Overview

The purpose of this project is to centralise control of URL access for a home network through DNS, blocking tracking requests, malicious websites as well as unwanted content (adult, gambling, drugs, violence etc.). 

### Key Benefits:
* **No Client Software:** As this is implemented through DNS, there is no need to configure endpoint devices with software or MDM, all devices, resident or guest, have the exact same enforcement rules applied; this prolongs endpoint battery life and reduces CPU usage, minimises administrative overhead and ensures peace of mind.
* **Privacy-Focused:** Blocks trackers and telemetry alongside harmful content.
* **Performance Boost:** As advertising media and URLs are blocked, webpages are able to be displayed faster.
* **Granular Control:** Whilst most ISP routers allow the use of upstream DNS servers such as OpenDNS FamilyShield for blocking harmful content, they don't provide any URL level control, opting instead for a everything or nothing approach. This solution grants parents complete control over everything and they can implement rules or changes as they see fit.
* **Logging:** Pi-hole's admin dashboard allows a user to see in real-time what resources are being requested and by whom. It allows parents to identify whether children may be attempting to circumvent restrictions, detect other resources which may need blocking and to understand problem areas - something not possible when using upstream DNS through an ISP router.

---

## The Hardware & Environment

* **Hardware:** Raspberry Pi or other SBC/ARM architecture devices or any 24/7 capable mini server, VM, literally anything that can run Pi-hole
* **Operating System:** for ease of installation and following this guide, I recommend a Raspberry Pi OS or Unix-based OS
* **Installation Method:** Terminal/CLI
* **Upstream DNS Provider:** 208.67.222.123 & 208.67.220.123 ([OpenDNS Family Shield](https://www.bing.com/ck/a?!&&p=57f180442291fddac6393f33b0bf6db08590621d15434d18c44ff05c92fe115cJmltdHM9MTc4MzM4MjQwMA&ptn=3&ver=2&hsh=4&fclid=10400cc6-3b86-6c91-2561-1b493a7d6d48&psq=opendns+family+shield&u=a1aHR0cHM6Ly9zaWdudXAub3BlbmRucy5jb20vZmFtaWx5c2hpZWxkLw))


---

## How it Works

Instead of modifying settings on individual devices, the network routes traffic seamlessly using the following flow:

1. **Device** requests a website (e.g., clicks a link).
2. **Router** is configured to point to the **Pi-hole** as its sole DNS server.
3. **Pi-hole** intercepts the request and cross-references it against the custom blocklists.
4. **Result:** If it's safe content, it resolves via the safe upstream DNS. If it's illicit or malware, the Pi-hole returns `0.0.0.0` (a dead end), and the content fails to load.

---

## Blocklist (Adlist) Configuration

To achieve reliable protection for kids without breaking the useful internet, I implemented a layered blocklist strategy:

| List Name / Source | Focus Area | Impact | Link |
| :--- | :--- | :--- | :--- |
| **StevenBlack FMS** | Pornography, Social Media, Fake News & Gambling | Restricts access to adult sites and gambling. | https://github.com/StevenBlack/hosts 
| **Hagezi DNS Blocklists** | Mixed Content | Block lists of varying degrees and a wide range of categories. | https://github.com/hagezi/dns-blocklists

> 💡 **Tip for families:** I also enabled **RegEx blocking** for specific high-risk keywords and explicitly blocked major malicious domains via the native blocklist settings.

---

##  Dashboard & Metrics

The interface allows me to audit what types of traffic are triggering blocks, ensuring the network stays clean without overly restricting normal web usage. 

