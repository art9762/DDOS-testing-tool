# Network Monitor - DoS Testing Tool

A Python web application for real-time network device discovery and packet tracking, designed for DoS algorithm testing and network security research.

## Features

- **Real-time Device Discovery**: Automatically discover devices on your network using ARP scanning
- **Packet Monitoring**: Track packets sent and received by each device with detailed statistics
- **Data Usage Tracking**: Monitor data transmission in GB for each connected device
- **Modern Web Interface**: Responsive design with real-time updates and interactive controls
- **Cross-Platform Support**: Works on MacBook Pro M4 and Raspberry Pi 4 Model B
- **REST API**: Full API access for integration with other tools
- **Auto-Scan Feature**: Continuous monitoring with configurable intervals

## Screenshots

The application features a modern, responsive web interface with:
- Network status indicators
- Device discovery controls
- Real-time statistics dashboard
- Interactive device cards with detailed information
- Toast notifications for user feedback

## Quick Start

### Prerequisites
- Python 3.8 or higher
- Administrator/sudo privileges (required for packet capture)
- Network interface access

### Installation
```bash
# Clone or download the application
cd network_monitor

# Set up virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run the application (requires sudo for packet capture)
sudo python src/main.py
```

### Access the Application
Open your browser and navigate to:
- Local: http://localhost:5001
- Network: http://[YOUR_IP]:5001

## Architecture

### Backend Components
- **Flask Web Framework**: RESTful API and web server
- **Scapy**: Network packet capture and analysis
- **Device Manager**: In-memory device tracking and statistics
- **Network Scanner**: ARP-based device discovery

### Frontend Components
- **Modern HTML5 Interface**: Responsive design with CSS Grid and Flexbox
- **Vanilla JavaScript**: Real-time API communication and DOM updates
- **CSS Animations**: Smooth transitions and interactive feedback
- **Font Awesome Icons**: Professional iconography

### Data Flow
1. Network scanner discovers devices using ARP requests
2. Packet monitor captures and analyzes network traffic
3. Device manager tracks statistics and maintains device state
4. Web interface provides real-time visualization and controls
5. REST API enables programmatic access to all functionality

## API Documentation

### Device Management
- `GET /api/network/devices` - Retrieve all discovered devices
- `GET /api/network/devices/online` - Get only online devices
- `GET /api/network/devices/<mac>/stats` - Get specific device statistics

### Network Operations
- `POST /api/network/scan` - Trigger network device discovery
- `GET /api/network/network-info` - Get network interface information

### Monitoring Controls
- `POST /api/network/monitoring/start` - Start packet monitoring
- `POST /api/network/monitoring/stop` - Stop packet monitoring
- `GET /api/network/monitoring/status` - Get monitoring status

### Auto-Scan Features
- `POST /api/network/auto-scan/start` - Enable automatic scanning
- `POST /api/network/auto-scan/stop` - Disable automatic scanning

### Data Management
- `POST /api/network/reset` - Clear all device data and statistics

## Device Information Tracked

For each discovered device, the application tracks:
- **Network Identifiers**: MAC address, IPv4, IPv6 addresses
- **Device Information**: Hostname, first seen, last seen timestamps
- **Traffic Statistics**: Packets sent/received, bytes transferred
- **Connection Status**: Online/offline status with timeout detection
- **Data Usage**: Total data transferred in GB

## Security Considerations

### Important Warnings
- **Requires Administrative Privileges**: Packet capture needs sudo/administrator access
- **Network Ownership**: Only use on networks you own or have explicit permission to monitor
- **Local Testing Only**: Designed for controlled environments and DoS algorithm development
- **No Authentication**: Web interface has no built-in authentication mechanism

### Recommended Security Practices
- Run only on isolated test networks
- Use firewall rules to restrict access
- Monitor application logs for unusual activity
- Regularly update dependencies for security patches

## Deployment Scenarios

### Development Environment (MacBook Pro M4)
Perfect for:
- DoS algorithm development and testing
- Network security research
- Educational purposes
- Proof-of-concept demonstrations

### Production Environment (Raspberry Pi 4)
Ideal for:
- Dedicated network monitoring appliance
- Continuous monitoring setups
- Remote network analysis
- IoT security testing

## Hotspot Configuration

While the application doesn't create hotspots programmatically, it works excellently with manually configured hotspots:

### MacBook Pro Hotspot
1. Enable "Internet Sharing" in System Preferences
2. Share connection via WiFi
3. Run the network monitor application
4. Connect test devices to the hotspot

### Raspberry Pi Access Point
1. Install and configure hostapd and dnsmasq
2. Set up WiFi access point with custom SSID
3. Configure DHCP for connected devices
4. Run network monitor to track all connections

## Performance Characteristics

### Resource Usage
- **Memory**: Approximately 50-100MB depending on device count
- **CPU**: Low impact during normal operation, higher during active scanning
- **Network**: Minimal overhead for monitoring, burst activity during scans

### Scalability
- **Device Limit**: Tested with up to 50 concurrent devices
- **Packet Rate**: Handles typical home/office network traffic volumes
- **Storage**: In-memory storage scales with device count and monitoring duration

## Troubleshooting

### Common Issues and Solutions

#### Permission Errors
```bash
# Ensure sudo privileges
sudo python src/main.py
```

#### No Devices Discovered
- Verify network connectivity
- Check active network interface
- Ensure devices are actually connected
- Try multiple scan attempts

#### Packet Monitoring Failures
- Confirm sudo/administrator privileges
- Verify network interface supports monitoring mode
- Check for conflicting network monitoring tools

#### Web Interface Not Loading
- Verify Flask is running on correct port
- Check firewall settings
- Ensure no port conflicts exist

## Development and Customization

### Code Structure
```
network_monitor/
├── src/
│   ├── main.py              # Flask application entry point
│   ├── models/
│   │   └── device.py        # Device data models
│   ├── routes/
│   │   └── network.py       # API route handlers
│   ├── network_scanner.py   # Network scanning logic
│   └── static/
│       ├── index.html       # Web interface
│       ├── styles.css       # Styling and animations
│       └── script.js        # Frontend JavaScript
└── requirements.txt         # Python dependencies
```

### Extending Functionality
- Add database persistence for long-term data storage
- Implement user authentication and authorization
- Create additional visualization charts and graphs
- Add export functionality for data analysis
- Integrate with external security tools

### Configuration Options
- Modify scan intervals in `src/routes/network.py`
- Customize network interface detection in `src/network_scanner.py`
- Adjust web interface styling in `src/static/styles.css`
- Configure packet capture filters in Scapy components

## License and Legal

This software is provided for educational and research purposes. Users must:
- Comply with local laws regarding network monitoring
- Obtain proper authorization before monitoring networks
- Use responsibly for legitimate security testing only
- Respect privacy and data protection regulations

## Support and Contributing

This is a specialized tool for DoS algorithm testing and network security research. For issues or enhancements:
- Review the troubleshooting section
- Check the deployment instructions
- Examine the API documentation
- Consider the security implications of any modifications

## Technical Specifications

### Supported Platforms
- **macOS**: macOS 10.15+ (tested on M4 MacBook Pro)
- **Linux**: Ubuntu 20.04+, Raspberry Pi OS
- **Windows**: Windows 10+ (with appropriate Python setup)

### Network Protocols
- **ARP**: Device discovery and MAC address resolution
- **IPv4/IPv6**: Dual-stack network support
- **Ethernet**: Wired network monitoring
- **WiFi**: Wireless network monitoring

### Dependencies
- **Flask**: Web framework and API server
- **Scapy**: Packet capture and network analysis
- **psutil**: System and network interface information
- **Flask-CORS**: Cross-origin request support

This application provides a comprehensive foundation for network monitoring and DoS testing research while maintaining focus on security, performance, and usability.

