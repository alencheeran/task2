 Task 2 — Phishing Email Analysis
Student Information
- **Name:** Alen C Bijoy  
- **Date:** October 2025  


Objective
Analyze a phishing email by examining its full headers and checking DNS, SPF, DKIM, and DMARC authentication results.  
Identify indicators of compromise (IOCs) and explain how attackers spoof legitimate organizations.

 Email Sample Details

**Subject:** CLIENTE PRIME - BRADESCO LIVELO: Seu cartão tem 92.990 pontos LIVELO expirando hoje!  
**From:** BANCO DO BRADESCO LIVELO `<banco.bradesco@atendimento.com.br>`  
**Return-Path:** `<root@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06>`  
**Message-ID:** `<20230919183549.39DEA3F725@ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06>`  
**Source IP:** `137.184.34.4`  
**Date:** Tue 19 Sep 2023 18:35:49 +0000 (UTC)

 Step-by-Step Header Analysis

 Relay Path
| Hop | From | By | Blacklist Status |
|:---:|:---|:---|:---|
| 1 | ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06 | – | – |
| 2 | 137.184.34.4 | BN8NAM11FT066.mail.protection.outlook.com | ⚠️ Blacklisted |
| 3 | BN8NAM11FT066.eop-nam11.prod.protection.outlook.com | BN0PR03CA0023.outlook.office365.com | ✅ Clean |
| 4 | BN0PR03CA0023.namprd03.prod.outlook.com | SA3PR19MB7370.namprd19.prod.outlook.com | ✅ Clean |
| 5 | SA3PR19MB7370.namprd19.prod.outlook.com | MN0PR19MB6312.namprd19.prod.outlook.com | ⚠️ Blacklisted |



DNS Analysis
- DNS lookup for `atendimento.com.br` **failed** — no valid DNS or mail-server records found.  
- DNS lookup for `ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06` **timed out**.  
- Indicates a **fake or misconfigured domain**, typical in phishing.

SPF (Sender Policy Framework)
**Result:** `spf=temperror (DNS Timeout)`  
- SPF verification failed for `ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06`.  
- IP `137.184.34.4` is **not authorized** to send for `atendimento.com.br`.  
- ✅ Legitimate Bradesco mail would use: `v=spf1 include:_spf.bradesco.com.br -all`.  
- ❌ **Conclusion:** SPF authentication failed.

 DKIM (DomainKeys Identified Mail)
**Result:** `No DKIM-Signature header found`  
- The message was **not cryptographically signed**.  
- Real financial institutions always sign mail with DKIM.  
- ❌ Missing DKIM signature → possible spoofing.

 DMARC (Domain-based Message Authentication, Reporting & Conformance)
**Result:** `No DMARC Record Found`  
- No DMARC policy at `_dmarc.atendimento.com.br`.  
- Without DMARC, mail servers can’t enforce actions on SPF/DKIM failures.  
- ❌ **Conclusion:** Domain unprotected from spoofing.


 Microsoft Composite Authentication
`compauth=fail reason=001`  
- Microsoft evaluation = **failed authentication**.  
- Marked as **unverified and risky**.



**Risk Level:** 🔴 High  
**Type:** Phishing / Spoofing  
**Recommended Action:** Quarantine or block immediately.

---

 Tools Used
- MXToolbox – Email header and DNS analyzer  
- VirusTotal / urlscan.io – Domain & IP reputation  
- Header Analyzer – SPF, DKIM, DMARC inspection  
- GitHub Desktop – Version control and submission  
