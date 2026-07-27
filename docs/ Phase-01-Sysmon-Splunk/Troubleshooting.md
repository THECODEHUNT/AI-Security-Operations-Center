# Troubleshooting Guide

## Overview

This document describes the verification steps and troubleshooting activities performed during **Phase 01 – Sysmon + Splunk Cloud Integration**.

The purpose of these checks was to ensure that Sysmon was generating endpoint telemetry correctly and that the collected events were successfully forwarded to Splunk Cloud.

---

# Issue 1 — Verifying Sysmon Event Generation

### Observation

After installing Sysmon, it was important to verify that endpoint events were being generated correctly before configuring log forwarding.

### Verification Steps

- Confirmed that the Sysmon service was running.
- Opened the Sysmon Operational log in Windows Event Viewer.
- Verified that Process Creation events were being recorded successfully.

### Outcome

Sysmon was successfully generating endpoint telemetry and writing events to the Windows Event Log.

---

# Issue 2 — Splunk Cloud Not Receiving Logs

### Observation

After configuring Splunk Universal Forwarder, Sysmon events did not initially appear in Splunk Cloud.

### Possible Cause

- Incorrect `inputs.conf` configuration.
- Splunk Universal Forwarder service required verification.

### Resolution

- Reviewed the `inputs.conf` configuration.
- Restarted the Splunk Universal Forwarder service.
- Verified that the correct Windows Event Log was being monitored.

### Outcome

Sysmon events were successfully forwarded to Splunk Cloud.

---

# Issue 3 — Service Permission Issue

### Observation

The Splunk Universal Forwarder service was unable to access the Windows Event Log using its current configuration.

### Cause

The service was not running with sufficient permissions to read the Sysmon Operational log.

### Resolution

The Splunk Universal Forwarder service was updated to run under the **Local System** account.

### Outcome

After updating the service account, the forwarder successfully collected and forwarded Sysmon events.

---

# Issue 4 — Validating Forwarded Events

### Observation

After logs started appearing in Splunk Cloud, the collected telemetry was validated to ensure that important Sysmon events were available for investigation.

### Validation Steps

- Generated Process Creation activity.
- Generated Network Connection activity.
- Searched for **Event ID 1** and **Event ID 3** in Splunk Cloud.

### Outcome

Both Event ID 1 and Event ID 3 were successfully searchable, confirming that endpoint telemetry was being forwarded correctly.

---

# Troubleshooting Summary

The troubleshooting activities documented in this guide helped verify the Sysmon installation, resolve log forwarding issues, correct service permissions, and confirm successful event collection in Splunk Cloud.

Completing these checks ensured that the endpoint telemetry pipeline was functioning as expected and was ready for the next phases of the AI Security Operations Center project.
