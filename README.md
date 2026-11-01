## Active Directory Security Monitoring Lab

### Overview

This project involved building a simulated Active Directory environment in Microsoft Azure and integrating it with Microsoft Sentinel for security monitoring and threat detection.

A Windows Server 2025 virtual machine was configured as a Domain Controller using Active Directory Domain Services (AD DS).

Windows security events were collected using the Azure Monitor Agent (AMA) and a Data Collection Rule (DCR), then sent to a Log Analytics workspace for analysis.

Microsoft Sentinel was used to develop analytics rules to detect suspicious activities, including repeated failed logon attempts, user account creation and security group membership changes.

### Objectives

- Deploy and configure an Active Directory Domain Controller in Azure.
- Create and manage test users and security groups.
- Configure Windows security event collection using Azure Monitor Agent.
- Integrate security logs with Microsoft Sentinel.
- Develop KQL queries for security event investigation.
- Create analytics rules to detect suspicious Active Directory activity.
- Map relevant detections to the MITRE ATT&CK framework.

## Architecture

The lab was deployed in Microsoft Azure using the following components:

| Component | Purpose |
|---|---|
| Azure Virtual Network (VNet) | Provides network connectivity for the lab resources. |
| Windows Server 2025 VM (`vm-ad-dc01`) | Acts as the Active Directory Domain Controller. |
| Active Directory Domain Services (AD DS) | Manages domain users, groups and authentication. |
| Azure Monitor Agent (AMA) | Collects Windows security events. |
| Data Collection Rule (DCR) | Defines which Windows security events are collected. |
| Log Analytics Workspace (`law-soc-lab`) | Stores collected security event data. |
| Microsoft Sentinel | Provides SIEM monitoring, analytics rules and incident investigation. |
| Azure Bastion | Provides secure browser-based access to the virtual machine. |

### Data Collection Flow

1. Windows security events are generated on the Domain Controller.
2. Azure Monitor Agent collects the selected events.
3. The Data Collection Rule controls which events are collected.
4. Events are sent to the Log Analytics Workspace.
5. Microsoft Sentinel analyses the data using KQL queries and analytics rules.
6. Matching activity can generate security alerts and incidents.

## Windows Security Events Monitored

The Data Collection Rule (`dcr-ad-security-lab`) was configured to collect selected Windows Security events from the Domain Controller.

| Event ID | Description | Security relevance |
|---|---|---|
| 4625 | Failed logon attempt | Identifies repeated authentication failures. |
| 4720 | User account created | Identifies new Active Directory accounts. |
| 4728 | Member added to a global security group | Monitors global group membership changes. |
| 4732 | Member added to a local security group | Monitors local group membership changes. |
| 4756 | Member added to a universal security group | Monitors universal group membership changes. |

These events provide visibility into authentication activity, account creation and changes to group membership within the Active Directory environment.

## Microsoft Sentinel Detection Rules

Four scheduled analytics rules were configured in Microsoft Sentinel to monitor Active Directory security activity.

### 1. Repeated Failed Logon Detection

- **Event ID:** 4625
- **Purpose:** Detects repeated failed logon attempts.
- **Detection logic:** Identifies three or more failed logon attempts within a five-minute period.
- **MITRE ATT&CK:** T1110 – Brute Force
- **Testing:** Tested by generating multiple incorrect password attempts.

### 2. User Account Creation Detection

- **Event ID:** 4720
- **Purpose:** Detects the creation of new Active Directory user accounts.
- **MITRE ATT&CK:** T1136 – Create Account
- **Testing:** Tested by creating a new test account.

### 3. Security Group Membership Change Detection

- **Event IDs:** 4728 and 4732
- **Purpose:** Detects when users are added to global or local security groups.
- **MITRE ATT&CK:** T1098 – Account Manipulation
- **Testing:** Tested by adding test accounts to `SOC-Test-Group`.

### 4. Privileged Group Membership Change Detection

- **Event IDs:** 4728, 4732 and 4756
- **Purpose:** Detects additions to sensitive groups such as Domain Admins, Enterprise Admins, Schema Admins and Administrators.
- **MITRE ATT&CK:** T1098 – Account Manipulation
- **Testing:** Detection query and group-name matching logic validated. A real privileged-group membership change has not yet been generated to trigger the rule.

### Tools and Technologies

| Tool / Technology | Usage |
|---|---|
| Microsoft Azure | Cloud infrastructure and virtual machine deployment. |
| Windows Server 2025 | Operating system for the Domain Controller. |
| Active Directory Domain Services | Identity and access management. |
| Microsoft Sentinel | Security monitoring and threat detection. |
| Azure Monitor Agent | Windows security event collection. |
| Log Analytics Workspace | Security log storage and analysis. |
| Kusto Query Language (KQL) | Querying and investigating security events. |
| Azure Bastion | Secure remote access to the Domain Controller. |
| MITRE ATT&CK | Mapping detections to recognised attack techniques. |

### Testing and Investigation

The lab was tested by generating controlled security events within the Active Directory environment.

The following activities were performed:

- Generated repeated failed logon attempts to test authentication monitoring.
- Created test user accounts to generate Event ID 4720.
- Added test users to `SOC-Test-Group` to generate Event ID 4728.
- Queried Windows security events in Log Analytics using KQL.
- Verified that the corresponding Microsoft Sentinel analytics rules generated alerts and incidents for the tested activities.
- Validated the privileged-group detection query without granting test accounts privileged access.

These tests demonstrated that security events could be collected, queried and used to generate alerts and incidents in Microsoft Sentinel.

### Findings and Learning Outcomes

This project provided practical experience in building and monitoring an Active Directory environment using Microsoft Azure and Microsoft Sentinel.

Key outcomes included:

- Successfully deployed and configured an Active Directory Domain Controller.
- Integrated Windows security event collection with Microsoft Sentinel.
- Used KQL to extract and investigate account, group and authentication activity.
- Configured four scheduled analytics rules for Active Directory security monitoring.
- Generated and investigated security alerts and incidents from controlled test activities.
- Gained practical experience with SIEM configuration, log analysis and detection engineering.

### Future Improvements

- Generate and investigate additional Active Directory security events.
- Expand monitoring to include more privileged account and group activities.
- Develop additional KQL detection queries.
- Improve alert investigation and incident documentation.
- Explore automated incident response using Microsoft Sentinel playbooks.

### Screenshots and Evidence

Screenshots demonstrating the lab configuration, security event collection and Microsoft Sentinel detection results will be included below.

### 1. Azure Infrastructure

![Azure Virtual Machine](screenshots/01-azure-vm.png)

### 2. Active Directory Configuration

![Active Directory Users](screenshots/03-active-directory-users.png)

![Security Group](screenshots/04-security-group.png)

### 3. Log Collection

![Data Collection Rule](screenshots/05-data-collection-rule.png)

### 4.  Microsoft Sentinel Detection

![Sentinel Analytics Rules](screenshots/07-sentinel-analytics-rules.png)

![Sentinel Incidents](screenshots/08-sentinel-incidents.png)

### 5. Security Investigation

![Failed Logon Investigation](screenshots/09-failed-logon-investigation.png)

![Account Creation Investigation](screenshots/10-account-creation-investigation.png)



