# PHOENIX - Post-Disaster Survivor Tracking System

![PHOENIX Banner](https://img.shields.io/badge/Hackathon-Omnikon-blue) ![Status](https://img.shields.io/badge/Status-In%20Development-yellow) ![License](https://img.shields.io/badge/License-MIT-green)

## Overview

**PHOENIX** is a real-time missing-person search coordination system designed for post-disaster scenarios. When disasters strike—earthquakes, floods, building collapses—search and rescue operations are often chaotic, uncoordinated, and lack real-time survivor health data. PHOENIX solves this by providing rescuers with a live dashboard that shows survivor locations, health vitals, proximity estimates, and emergency alerts—all without depending on cellular networks or internet connectivity.

## Problem Statement

**Real-Time Missing-Person Search Coordination**

Search for missing persons after disasters is often disorganized across multiple agencies and volunteers. PHOENIX creates a solution that coordinates this search more effectively by:
- Providing real-time survivor tracking and health monitoring
- Enabling communication in network-dead zones using LoRa technology
- Prioritizing rescue operations based on health severity and proximity
- Coordinating multiple rescue teams with a centralized command center

## How It Works

PHOENIX consists of two main components:

### 1. Survivor Device (Wearable/Portable)
- **Health Monitoring**: Continuously tracks heart rate, SpO₂ (oxygen saturation), motion, and altitude
- **LoRa Communication**: Sends distress signals and health data over long distances (up to 2-5 km) without cellular networks
- **Emergency Detection**: Automatically detects critical health conditions (cardiac arrest, hypoxia, no motion)
- **Low Power**: Operates on battery power with efficient energy management

### 2. Rescue Device + Command Center
- **Mobile Rescue Unit**: ESP32-based device carried by rescue teams with GPS, LoRa receiver, and OLED display
- **Real-Time Dashboard**: Web-based command center showing:
  - Live survivor locations on a map
  - Health vitals and emergency alerts
  - Signal strength (RSSI) for proximity estimation
  - Priority scoring for rescue operations
- **Multi-Team Coordination**: Supports multiple rescue devices feeding data to a central command

## Key Features

✅ **Network-Independent**: Works without cellular or internet connectivity using LoRa  
✅ **Real-Time Health Monitoring**: Heart rate, SpO₂, motion, pressure/altitude  
✅ **Emergency Detection**: Automatic alerts for cardiac arrest, hypoxia, falls  
✅ **Proximity Estimation**: Uses RSSI (signal strength) to guide rescuers  
✅ **Priority Engine**: Scores survivors based on health severity and proximity  
✅ **Centralized Dashboard**: Command center for coordinating multiple rescue teams  
✅ **Low Cost**: Built with affordable, accessible hardware components  

## Tech Stack

### Hardware
| Component | Model | Purpose |
|-----------|-------|---------|
| Main MCU | ESP32 | Microcontroller for both survivor and rescue devices |
| Communication | SX1278 LoRa | Long-range radio communication (2-5 km) |
| Heart Rate & SpO₂ | MAX30102 | Pulse oximeter sensor |
| Motion Sensor | MPU6050 | Accelerometer and gyroscope |
| Pressure/Altitude | BMP280 | Barometric pressure sensor |
| GPS | NEO-6M | Location tracking for rescue device |
| Display | 0.96" OLED | Real-time data display for rescuers |

### Software
| Layer | Technology | Purpose |
|-------|-----------|---------|
| Firmware | C++ (Arduino/PlatformIO) | Sensor interfacing, LoRa communication |
| Backend | Python + FastAPI | API server for rescue device ↔ dashboard |
| Database | SQLite | Local storage of survivor data, health vitals, GPS |
| Dashboard | HTML + CSS + JavaScript | Command center web interface |
| Communication | Wi-Fi (Rescue → Command) | Rescue device sends data to command center |
| Version Control | Git/GitHub | Source code management |

### Development Tools
- **IDE**: VS Code with PlatformIO / Arduino IDE
- **Version Control**: Git/GitHub

## Architecture

```
┌─────────────────────┐
│  Survivor Device    │
│  (Wearable/Portable)│
│                     │
│  ┌───────────────┐  │
│  │ MAX30102      │  │  Heart Rate, SpO₂
│  │ MPU6050       │  │  Motion, Orientation
│  │ BMP280        │  │  Altitude, Pressure
│  │ ESP32 + LoRa  │  │  Data Processing & Transmission
│  └───────────────┘  │
└──────────┬──────────┘
           │ LoRa (2-5 km, no network needed)
           ↓
┌─────────────────────┐
│  Rescue Device      │
│  (Mobile Unit)      │
│                     │
│  ┌───────────────┐  │
│  │ ESP32 + LoRa  │  │  Receive survivor data
│  │ NEO-6M GPS    │  │  Rescue team location
│  │ OLED Display  │  │  Show survivor info
│  └───────────────┘  │
└──────────┬──────────┘
           │ Wi-Fi
           ↓
┌─────────────────────┐
│  Command Center     │
│  (Web Dashboard)    │
│                     │
│  ┌───────────────┐  │
│  │ FastAPI       │  │  Backend API
│  │ SQLite DB     │  │  Data storage
│  │ Web Dashboard │  │  Live monitoring
│  └───────────────┘  │
└─────────────────────┘
```

## Setup Instructions

### Prerequisites

**Hardware:**
- ESP32 development boards (2x - one for survivor, one for rescue)
- SX1278 LoRa modules (2x)
- MAX30102 pulse oximeter sensor
- MPU6050 IMU sensor
- BMP280 pressure sensor
- NEO-6M GPS module
- 0.96" OLED display (I2C)
- Jumper wires, breadboard, power supply

**Software:**
```bash
# Required software versions
Python >= 3.8
PlatformIO or Arduino IDE
Git
```

### Installation

#### 1. Clone the Repository

```bash
git clone https://github.com/sarishanasane-beep/omnikon-hackathon.git
cd omnikon-hackathon
```

#### 2. Firmware Setup (ESP32 Devices)

```bash
# Open the firmware folder in PlatformIO or Arduino IDE
cd firmware

# Install required libraries via PlatformIO:
# - RadioLib (LoRa communication)
# - MAX30102
# - MPU6050
# - Adafruit BMP280
# - TinyGPS++

# Configure LoRa parameters in config.h
# Set survivor device ID, frequency, spreading factor

# Upload to ESP32 boards:
# - survivor_device.cpp → Survivor wearable
# - rescue_device.cpp → Rescue unit
```

#### 3. Backend Setup

```bash
cd backend

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install fastapi uvicorn sqlite3

# Run the API server
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

#### 4. Dashboard Setup

```bash
cd dashboard

# Open index.html in a web browser
# Or serve with a local server:
python -m http.server 8080

# Access dashboard at: http://localhost:8080
```

### Configuration

1. **LoRa Settings** (`config.h`):
   - Frequency: 433 MHz (or regional frequency)
   - Spreading Factor: 7-12 (range vs speed tradeoff)
   - Bandwidth: 125 kHz
   - Coding Rate: 4/5

2. **Survivor Device**:
   - Set unique device ID
   - Configure health thresholds (heart rate, SpO₂)
   - Set transmission interval (default: 5 seconds)

3. **Rescue Device**:
   - Set Wi-Fi credentials for command center connection
   - Configure API endpoint URL

### Running the System

#### Survivor Device:
```bash
# Power on the ESP32 with sensors
# Device will start monitoring health and transmitting via LoRa
# LED indicators show transmission status
```

#### Rescue Device:
```bash
# Power on the ESP32 with GPS and OLED
# Connect to Wi-Fi
# OLED displays survivor data in real-time
```

#### Command Center:
```bash
# Ensure backend API is running (port 8000)
# Open dashboard in browser
# Monitor all survivors and rescue teams in real-time
```

## Usage

### For Rescue Coordinators

1. **Monitor Dashboard**: View all active survivors, their health vitals, and locations
2. **Check Alerts**: Red alerts indicate critical health emergencies
3. **Prioritize Rescues**: Use priority scores to dispatch teams efficiently
4. **Track Teams**: See rescue team locations via GPS
5. **View History**: Access historical health data for each survivor

### For Rescue Teams

1. **Carry Rescue Device**: Keep powered on and connected to command center
2. **Check OLED Display**: Shows nearest survivor info and distance estimate
3. **Follow RSSI Signal**: Higher signal strength = closer to survivor
4. **Report Status**: Device automatically sends GPS location to command

## API Documentation

### Endpoints

#### `POST /api/survivor/update`
Upload survivor health data from rescue device
```json
{
  "survivor_id": "SV001",
  "timestamp": 1692134400,
  "heart_rate": 85,
  "spo2": 96,
  "motion_detected": true,
  "altitude": 1205.5,
  "rssi": -75
}
```

#### `GET /api/survivors`
Retrieve all active survivors and their latest data

#### `GET /api/survivor/{id}`
Get detailed info and history for specific survivor

#### `GET /api/rescue/teams`
Get locations of all rescue teams

#### `GET /api/alerts`
Get active emergency alerts

## Team Members & Contributions

| Name | Role | Contributions |
|------|------|---------------|
| **Sarish Anasane** | Hardware & Firmware Lead | ESP32 firmware development, sensor integration, LoRa communication, hardware assembly, testing |
| **Arya Ghate** | Backend & Dashboard Lead | FastAPI backend development, SQLite database design, web dashboard UI/UX, API integration |

## Third-Party Attributions

This project uses the following libraries and frameworks:

### Firmware Libraries
- [Arduino ESP32 Framework](https://github.com/espressif/arduino-esp32) - ESP32 microcontroller support
- [RadioLib](https://github.com/jgromes/RadioLib) - SX1278 LoRa communication
- [MAX30102 Library](https://github.com/sparkfun/SparkFun_MAX3010x_Sensor_Library) - Pulse oximeter sensor
- [MPU6050 Library](https://github.com/electroniccats/mpu6050) - IMU sensor interface
- [Adafruit BMP280](https://github.com/adafruit/Adafruit_BMP280_Library) - Pressure/altitude sensor
- [TinyGPS++](https://github.com/mikalhart/TinyGPSPlus) - GPS data parsing

### Backend & Database
- [FastAPI](https://fastapi.tiangolo.com/) - Modern Python web framework
- [SQLite](https://www.sqlite.org/) - Lightweight embedded database
- [Uvicorn](https://www.uvicorn.org/) - ASGI server

### Development Tools
- [PlatformIO](https://platformio.org/) - Embedded development platform
- [Git/GitHub](https://github.com/) - Version control

### Hardware Components
- **Espressif ESP32** - Microcontroller
- **Semtech SX1278** - LoRa transceiver
- **Maxim MAX30102** - Pulse oximeter sensor
- **InvenSense MPU6050** - IMU sensor
- **Bosch BMP280** - Pressure sensor
- **u-blox NEO-6M** - GPS module

**Note:** PHOENIX does not use external AI APIs, cloud services, payment APIs, or authentication services in the current prototype. All processing is done locally.

## Project Timeline

- **Hackathon Registration**: August 2026
- **Development Start**: August 2026
- **Current Status**: Active Development
- **Target**: Round 2 Submission with Live Demo

## Demo

**Live Demo:** Coming Soon (Required by Round 2)

**Demo Video:** TBD

## Screenshots

_Coming Soon - Screenshots of Command Center Dashboard and Rescue Device Display_

## Roadmap

- [x] Hardware prototype assembly
- [x] ESP32 firmware for survivor device
- [x] ESP32 firmware for rescue device
- [x] LoRa communication implementation
- [x] Sensor integration (MAX30102, MPU6050, BMP280, GPS)
- [x] Backend API development
- [x] SQLite database design
- [ ] Web dashboard completion
- [ ] Testing in simulated disaster scenario
- [ ] Battery optimization
- [ ] Deployment and live demo
- [ ] Documentation finalization

## Future Enhancements

- **Stage 2**: Add mapping API integration for visual location display
- **Stage 3**: Multi-hop LoRa mesh network for extended range
- **Stage 4**: Machine learning for predictive health analytics
- **Stage 5**: Mobile app for rescue teams
- **Stage 6**: Integration with existing emergency response systems

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct and the process for submitting pull requests.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Security

Please read [SECURITY.md](SECURITY.md) for information about our data handling and security practices.

## Acknowledgments

- Omnikon Hackathon organizers
- Open source community for libraries and frameworks
- Disaster response professionals for domain insights

## Contact

For questions or feedback, reach out to:

**Team Byte Benders**

- **Sarish Anasane**: sarishanasane@gmail.com | +91 9511231195
- **Arya Ghate**: aryaghate11@gmail.com | +91 7385749059

**GitHub Issues**: [Report Issues](https://github.com/sarishanasane-beep/omnikon-hackathon/issues)

---

**Built with ❤️ for Omnikon Hackathon 2026**

**Saving lives through technology.**
