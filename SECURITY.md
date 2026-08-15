# Security Policy

## Data Handling and Protection

This document outlines how PHOENIX handles and protects data during disaster response operations.

## Data Collection

### What Data We Collect

**Survivor Device Data:**
- **Health Vitals**: Heart rate (BPM), SpO₂ (blood oxygen saturation %), body temperature
- **Motion Data**: Accelerometer and gyroscope readings (motion detection, fall detection)
- **Environmental Data**: Barometric pressure, estimated altitude
- **Device Metadata**: Survivor device ID, timestamp, battery level
- **Signal Strength**: RSSI (Received Signal Strength Indicator) for proximity estimation

**Rescue Device Data:**
- GPS coordinates (latitude, longitude, altitude)
- Rescue team device ID
- Timestamp of data collection

**No PII (Personally Identifiable Information)** is collected in the current prototype. Survivor devices are identified by anonymous device IDs (e.g., "SV001"), not by names or personal details.

### How We Collect Data

- **Automatic Sensor Polling**: Survivor devices continuously monitor health sensors (every 5 seconds by default)
- **LoRa Transmission**: Data is transmitted wirelessly via LoRa radio (433 MHz, no internet required)
- **Rescue Device Reception**: Rescue units receive LoRa packets and forward to Command Center
- **No User Consent Required**: This is an emergency response system; data collection begins automatically when device is activated

## Data Storage

### Storage Methods

**Local Storage (Survivor Device):**
- ESP32 Flash Memory / LittleFS filesystem
- Stores recent health readings in circular buffer (RAM)
- Data retained temporarily until transmitted
- **Not encrypted** (resource-constrained embedded device)

**Database (Command Center):**
- **SQLite** database on local server (not cloud-hosted)
- Stores: Survivor vitals, rescue team GPS, RSSI history, emergency alerts
- Location: `backend/phoenix.db` (local file)
- **Encryption at rest**: Not implemented in current prototype (local deployment only)

**In-Transit:**
- LoRa packets are **not encrypted** in current prototype (performance/complexity tradeoff)
- Wi-Fi connection between rescue device and command center uses **WPA2/WPA3** encryption
- HTTP API communication is local network only (not exposed to internet)

### Data Retention

- **Real-Time Operation**: Data is retained only for the duration of the rescue operation
- **Historical Records**: Command Center stores historical data for post-operation analysis
- **Automatic Deletion**: No automatic deletion policy in current prototype
- **Manual Cleanup**: Database can be cleared manually after operation concludes

## Data Security Measures

### Encryption

- **In Transit (LoRa)**: ⚠️ **Not encrypted** in current prototype (future enhancement planned)
- **In Transit (Wi-Fi)**: Protected by WPA2/WPA3 Wi-Fi encryption
- **At Rest (Database)**: ⚠️ **Not encrypted** (local deployment, controlled environment)

### Authentication & Authorization

- **No Authentication Required**: This is an emergency system; command center dashboard has no login
- **Physical Security**: Command center server is physically secured in rescue operations base
- **Network Isolation**: System operates on local network, not connected to public internet during operations

### API Security

- **Local Network Only**: FastAPI backend accessible only on local network (127.0.0.1 or LAN)
- **No API Keys Required**: Internal system, no external API access
- **Input Validation**: Sensor data validated for expected ranges (e.g., heart rate 0-255 BPM)
- **Rate Limiting**: Not implemented (controlled device count, trusted source)

### Environment Variables

- **Wi-Fi Credentials**: Stored in ESP32 firmware configuration (not in public repository)
- **No Secrets in Code**: Sensitive configuration excluded from Git via `.gitignore`
- **`.env.example`**: Template provided without actual credentials

## Third-Party Services

### External APIs Used

| Service | Purpose | Data Shared | Privacy Policy |
|---------|---------|-------------|----------------|
| **None** | PHOENIX operates independently without external APIs | N/A | N/A |

**Note:** PHOENIX does not use cloud services, AI APIs, authentication providers, or external databases. All processing and storage is local.

### Data Processing Agreements

- Not applicable (no third-party data processors)

## Compliance

- **GDPR/CCPA**: Not applicable in current prototype (emergency system, anonymous IDs, no PII)
- **HIPAA**: Not compliant (health data stored unencrypted; not intended for clinical use)
- **Emergency Use Case**: Designed for disaster scenarios where traditional privacy regulations may be relaxed under emergency powers

### User Rights

Since this is an emergency response system with anonymous device IDs:
- **Data Access**: Command center operators can view all survivor data
- **Data Deletion**: Not supported during active rescue operations
- **Data Portability**: SQLite database can be exported for analysis

## Security Best Practices

### For Contributors

1. **Never commit secrets**: Use `.env` files and ensure `.gitignore` excludes them
2. **Validate all inputs**: Sanitize sensor data to prevent buffer overflows
3. **Bounds checking**: Verify sensor readings are within expected ranges
4. **Keep dependencies updated**: Regularly update libraries to patch vulnerabilities
5. **Review code for security issues**: Check for memory leaks, buffer overflows before merging
6. **Test on hardware**: Verify firmware stability under real conditions

### For Deployment Teams

1. **Secure Wi-Fi Network**: Use WPA3 encryption and strong passwords
2. **Physical Security**: Protect command center hardware from unauthorized access
3. **Isolate Network**: Do not connect command center to public internet during operations
4. **Battery Backup**: Ensure uninterrupted operation during power outages
5. **Regular Testing**: Test LoRa range and sensor accuracy before deployment

### For Rescue Operators

1. **Handle Devices Carefully**: Sensors are calibrated and fragile
2. **Monitor Battery Levels**: Replace batteries in survivor devices when low
3. **Verify Data Accuracy**: Cross-check sensor readings with visual assessment
4. **Report Anomalies**: Flag erratic sensor readings to technical team

## Known Limitations

### Security Limitations

⚠️ **LoRa Communication Not Encrypted**: Current prototype transmits health data unencrypted over LoRa radio. This is a known limitation due to:
- Resource constraints on low-power ESP32 devices
- Need for maximum transmission range and reliability
- Acceptable risk in disaster scenarios where speed > privacy

⚠️ **No Authentication**: Command center dashboard has no login system. Physical security of the server is critical.

⚠️ **Database Not Encrypted**: SQLite database stores data in plaintext. Acceptable for local deployment but not for long-term storage.

### Operational Limitations

- **Range**: LoRa range limited to 2-5 km in urban environments (line-of-sight required for max range)
- **Battery Life**: Survivor devices operate 12-24 hours on battery (needs field testing)
- **Sensor Accuracy**: MAX30102 pulse oximeter may be inaccurate on cold or moving fingers
- **GPS Accuracy**: NEO-6M GPS accurate to ~2.5 meters under clear sky (degraded in rubble)

### Future Security Enhancements

- [ ] Add AES-128 encryption for LoRa packets
- [ ] Implement HMAC for message authentication
- [ ] Add basic authentication to command center dashboard
- [ ] Encrypt SQLite database with SQLCipher
- [ ] Implement secure key exchange protocol for device pairing
- [ ] Add audit logging for all data access

## Reporting a Vulnerability

If you discover a security vulnerability, please follow these steps:

1. **Do NOT** open a public GitHub issue
2. Email the security team at: **sarishanasane@gmail.com**
3. Include:
   - Description of the vulnerability
   - Steps to reproduce
   - Potential impact (data exposure, device compromise, etc.)
   - Suggested fix (if any)
   - Your contact information for follow-up

### Response Timeline

- **Acknowledgment**: Within 48 hours
- **Initial Assessment**: Within 5 business days
- **Status Updates**: Every 7 days until resolved
- **Resolution**: As quickly as possible based on severity (critical issues prioritized)

### Severity Classification

- **Critical**: Remote code execution, full database access, encryption bypass
- **High**: Sensor data manipulation, unauthorized command center access
- **Medium**: Information disclosure, denial of service
- **Low**: Minor information leaks, non-security bugs

### Disclosure Policy

- We follow responsible disclosure practices
- Security patches will be released before public disclosure
- Credit will be given to researchers who report vulnerabilities responsibly
- We will coordinate disclosure timing with reporter

## Security Checklist for Hackathon Compliance

- [x] SECURITY.md file created ✅
- [x] No API keys in source code ✅
- [x] `.gitignore` excludes sensitive files ✅
- [x] Input validation implemented for sensor data ✅
- [x] Wi-Fi credentials stored in environment config (not in repo) ✅
- [x] Third-party libraries documented ✅
- [x] Known limitations documented ✅
- [ ] Encryption implemented (future enhancement)
- [ ] Authentication implemented (future enhancement)
- [ ] Security testing completed (in progress)
- [ ] Dependency security audit (`npm audit` or equivalent) - pending

## Updates

This security policy was last updated: **August 16, 2026**

We review and update this policy regularly. Check back for updates as the project evolves.

## Contact

For security-related questions or concerns:

**Team Byte Benders**
- **Email**: sarishanasane@gmail.com (Sarish Anasane, Technical Lead)
- **Phone**: +91 9511231195
- **GitHub Security Advisories**: [Report via GitHub](https://github.com/sarishanasane-beep/omnikon-hackathon/security)

---

**Important Notice:** PHOENIX is a hackathon prototype designed for emergency disaster response. While we follow security best practices where feasible, this is not a production-ready system. The design prioritizes rapid deployment, reliability, and life-saving functionality over enterprise-grade security. Use in actual disaster scenarios should be preceded by thorough security hardening and regulatory approval.

**This project is submitted for Omnikon Hackathon 2026.**
