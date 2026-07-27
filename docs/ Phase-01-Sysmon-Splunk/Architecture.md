# Architecture Overview

## Overview

This document explains the architecture used in **Phase 01 – Sysmon + Splunk Cloud Integration**.

The objective of this setup is to collect detailed endpoint telemetry from a Windows machine, forward it to Splunk Cloud, and make the data available for monitoring and security investigations.

Each component in the architecture has a specific role, working together to create a reliable endpoint logging pipeline.

---

# System Architecture

```text
+---------------------------+
|   Windows 10 Endpoint     |
+---------------------------+
              │
              ▼
+---------------------------+
|          Sysmon           |
+---------------------------+
              │
              ▼
+---------------------------+
|  Windows Event Log        |
| (Sysmon Operational Log)  |
+---------------------------+
              │
              ▼
+---------------------------+
| Splunk Universal Forwarder|
+---------------------------+
              │
              ▼
+---------------------------+
|      Splunk Cloud SIEM    |
+---------------------------+
              │
              ▼
+---------------------------+
| Search • Monitoring       |
| Investigation • Analysis  |
+---------------------------+
```

---

# Component Description

## 💻 Windows Endpoint

The Windows 10 virtual machine acts as the endpoint where user activities and system events are generated.

Every action performed on the system, such as launching an application or creating a network connection, becomes a potential security event.

---

## 🔍 Sysmon

**Sysmon (System Monitor)** is responsible for collecting detailed endpoint telemetry.

Unlike the default Windows logging, Sysmon records additional security information that is valuable during investigations.

Some of the important events captured include:

- Process Creation
- Network Connections
- DNS Queries
- File Creation
- Registry Modifications

All collected events are written to the Windows Event Log.

---

## 📋 Windows Event Log

Sysmon stores its events in the following location:

```text
Applications and Services Logs
└── Microsoft
     └── Windows
          └── Sysmon
               └── Operational
```

This Operational log acts as the primary source of endpoint telemetry before the data is forwarded to Splunk Cloud.

---

## 🚀 Splunk Universal Forwarder

The **Splunk Universal Forwarder** continuously monitors the Sysmon Operational log.

Whenever a new event is generated, the forwarder securely sends it to Splunk Cloud without requiring manual intervention.

Because it runs as a lightweight background service, it has minimal impact on system performance.

---

## ☁️ Splunk Cloud

**Splunk Cloud** is the centralized Security Information and Event Management (SIEM) platform used in this project.

After receiving endpoint telemetry, it provides a centralized interface for:

- Log Search
- Security Monitoring
- Event Correlation
- Threat Investigation
- Detection Engineering

This allows analysts to investigate endpoint activity from a single platform.

---

# Data Flow

The following sequence describes how endpoint telemetry moves through the environment.

1. A user performs an activity on the Windows endpoint.
2. Sysmon captures the activity and creates a security event.
3. The event is stored in the Windows Event Log.
4. Splunk Universal Forwarder detects the new event.
5. The event is securely forwarded to Splunk Cloud.
6. Analysts can search, monitor, and investigate the collected telemetry.

---

# Why This Architecture?

This architecture was designed to provide:

- Centralized log collection
- Continuous endpoint monitoring
- Better visibility into Windows activities
- Faster security investigations
- A scalable foundation for future SOC automation

---

# Architecture Summary

This architecture successfully connects a Windows endpoint with Splunk Cloud using Sysmon and Splunk Universal Forwarder.

The completed telemetry pipeline serves as the foundation for the upcoming phases of the AI Security Operations Center project, where the collected logs will be used for threat detection, alert generation, and incident response.
