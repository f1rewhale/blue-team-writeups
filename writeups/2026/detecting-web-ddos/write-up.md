# Detecting Web DDoS

**Platform:** TryHackMe  
**Difficulty:** Easy  
**Date:** 2026-09-19  

## Overview

Room focused on detecting and analyzing Application-Layer HTTP Flood (DDoS) attacks from a Blue Team perspective. The investigation is divided into two parts: direct command-line log analysis (`awk`, `grep`, `sort`, `uniq`) and SIEM aggregation using Splunk.

## What happened

An attacker targeted web application endpoints (`/login` and `/search`) with automated high-volume HTTP requests. The flood overwhelmed system resources, causing legitimate internal users to experience service outages represented by HTTP `503 Service Unavailable` status codes.

Key details:
- **CLI Attack IP:** `203.12.23.195` (targeting `/login`)
- **SIEM Attack URI:** `/search`
- **Top Botnet Client IP:** `203.0.113.7`
- **Attacking User-Agent:** `libwww-perl/6.05`
- **Impact Error Code:** `503`

## Blue Team Takeaways

**Detection ideas:**
- Sudden spikes in traffic targeting specific dynamic endpoints (e.g., `/login`, `/search`).
- High request frequency coming from single IP addresses or uncommon User-Agents (`curl`, `libwww-perl`).
- An unusually high count of `503 Service Unavailable` response codes indicating resource exhaustion.

**Hunting:**
- Group access logs by requesting IP and count frequency to expose top talkers (`uniq -c` or Splunk `top clientip`).
- Use timecharts with short spans (e.g., `span=1s`) to visualize micro-bursts in request rates.
- Cross-reference non-attacking client IP logs to verify legitimate user impact during incident windows.

**Mitigations:**
- Implement rate-limiting and CAPTCHA controls on resource-intensive endpoints.
- Deploy Web Application Firewall (WAF) rules to block known automated tool User-Agents.
- Set up automated SIEM alerts for high request-per-second thresholds and status 503 error rate spikes.

## Lessons Learned

Application-layer DDoS attacks do not always require massive network bandwidth to degrade availability. Targeting specific endpoints can easily deplete backend server threads. Systematic log parsing and SIEM queries allow SOC analysts to quickly isolate botnet IPs and measure user impact.

---

*Focused on defensive value.*
