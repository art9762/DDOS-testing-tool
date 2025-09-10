# Network Monitoring Application Architecture

## Overview
This application will monitor network devices connected to a local hotspot and track their packet transmission data. The architecture consists of:

1. **Backend (Python Flask)**: Network monitoring, device discovery, packet capture
2. **Frontend (Web Interface)**: Display device information and statistics
3. **Data Storage**: In-memory storage for real-time data

## Key Components

### 1. Network Monitoring Module
- **Library**: Scapy for packet capture and network scanning
- **Functions**:
  - Device discovery using ARP scanning
  - Packet capture and analysis
  - Data transmission tracking
  - Real-time statistics calculation

### 2. Web Backend (Flask)
- **Framework**: Flask with CORS support
- **Endpoints**:
  - `/api/devices` - Get list of connected devices
  - `/api/devices/<mac>/stats` - Get packet statistics for specific device
  - `/api/scan` - Trigger network scan
  - `/` - Serve web interface

### 3. Web Frontend
- **Technology**: HTML/CSS/JavaScript (vanilla or simple framework)
- **Features**:
  - Real-time device list display
  - Packet statistics visualization
  - Auto-refresh functionality

### 4. Data Structure
```python
{
    "devices": {
        "mac_address": {
            "ip4": "192.168.1.100",
            "ip6": "fe80::...",
            "mac": "aa:bb:cc:dd:ee:ff",
            "hostname": "device-name",
            "first_seen": "timestamp",
            "last_seen": "timestamp",
            "packets_sent": 1234,
            "packets_received": 5678,
            "bytes_sent": 1048576,
            "bytes_received": 2097152
        }
    }
}
```

## Required Libraries
- Flask (web framework)
- Flask-CORS (cross-origin requests)
- Scapy (network monitoring)
- psutil (system information)
- threading (background monitoring)

## Deployment Considerations
- **MacBook Pro M4**: Development and testing
- **Raspberry Pi 4**: Production deployment
- **Hotspot Setup**: Manual configuration using system utilities
- **Permissions**: May require sudo for packet capture

## Security Notes
- Application is for local testing only
- Packet capture requires elevated privileges
- Data is stored in memory (not persistent)

