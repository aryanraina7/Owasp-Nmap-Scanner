# 🔐 Automated Vulnerability Scanning Pipeline (OWASP ZAP + Nmap)

This repository provides an Azure DevOps pipeline that automates vulnerability scanning using [OWASP ZAP](https://www.zaproxy.org/) and [Nmap](https://nmap.org/) through Docker containers. The scans are designed to detect security issues in your web applications and network infrastructure as part of your CI/CD process.

## 📄 Pipeline Overview

The Azure DevOps pipeline runs the following in sequence:

1. **OWASP ZAP Scan**  
   - Performs a full active scan on the target URL.
   - Generates both HTML and XML vulnerability reports.
   - Publishes the reports as pipeline artifacts.

2. **Nmap Scans**  
   - Performs multiple types of scans including:
     - Service/version detection
     - Vulnerability script scan
     - Aggressive scan (`-A`)
   - Outputs results in plain text files.
   - Publishes each scan result as separate artifacts.

---

## 🧪 Prerequisites

### ✅ Azure DevOps Setup
- Self-hosted agent pool (configured with Docker).
- Variable group in Azure DevOps named `owaspvariables` containing the following:
  - `TARGET_URL`: The URL to scan using OWASP ZAP.
  - `HTML_REPORT_NAME`: Name of the HTML report file.
  - `XML_REPORT_NAME`: Name of the XML report file.
  - `nmapTarget`: IP address or hostname to be scanned by Nmap.
  - `outputFile1`, `outputFile2`, `outputFile3`, `outputFile4`: Output filenames for each Nmap scan.

### ✅ Agent Requirements
- Docker must be installed on the build agent.
- Sufficient permissions and network access to reach the scan targets.

---

## 🏗️ Pipeline Structure

```yaml
trigger:
  - main

pool:
  name: ARM

variables:
  - group: 'owaspvariables'

stages:
  - stage: VulnerabilityScanning
    jobs:
      - job: OWASPZAPScan
      - job: NmapScan
