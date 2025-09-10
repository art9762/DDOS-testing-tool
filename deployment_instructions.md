# Network Monitor - Deployment Instructions

## Overview
This Python web application provides real-time network device discovery and packet tracking capabilities for DoS algorithm testing. The application uses Flask for the backend and a modern web interface for visualization.

## System Requirements

### For MacBook Pro M4 (Development/Testing)
- Python 3.8 or higher
- pip package manager
- Administrator/sudo privileges (required for packet capture)
- Network interface access

### For Raspberry Pi 4 Model B (Production)
- Raspberry Pi OS (Debian-based)
- Python 3.8 or higher
- pip package manager
- sudo privileges
- Network interface access

## Installation Steps

### 1. Clone/Copy the Application
```bash
# Copy the network_monitor directory to your target machine
# Ensure all files are present:
# - src/main.py
# - src/models/device.py
# - src/network_scanner.py
# - src/routes/network.py
# - src/static/index.html
# - src/static/styles.css
# - src/static/script.js
# - requirements.txt
```

### 2. Set Up Python Virtual Environment
```bash
cd network_monitor
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Configure Network Interface (Important)
The application automatically detects your network interface, but you may need to:

#### On MacBook Pro:
- Ensure WiFi or Ethernet is connected
- The app will use the active interface automatically

#### On Raspberry Pi:
- For WiFi hotspot mode, configure the Pi as an access point first:
```bash
# Install hostapd and dnsmasq
sudo apt update
sudo apt install hostapd dnsmasq

# Configure hostapd
sudo nano /etc/hostapd/hostapd.conf
# Add:
interface=wlan0
driver=nl80211
ssid=NetworkMonitor
hw_mode=g
channel=7
wmm_enabled=0
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=YourPassword
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP

# Configure dnsmasq
sudo nano /etc/dnsmasq.conf
# Add:
interface=wlan0
dhcp-range=192.168.4.2,192.168.4.20,255.255.255.0,24h

# Enable services
sudo systemctl enable hostapd
sudo systemctl enable dnsmasq
```

## Running the Application

### Development Mode (MacBook Pro)
```bash
cd network_monitor
source venv/bin/activate
sudo python src/main.py  # sudo required for packet capture
```

### Production Mode (Raspberry Pi)
```bash
cd network_monitor
source venv/bin/activate
sudo python src/main.py
```

The application will be available at:
- Local access: http://localhost:5001
- Network access: http://[YOUR_IP]:5001

## Usage Instructions

### 1. Initial Setup
1. Open the web interface in your browser
2. The application will automatically detect your network interface
3. Network information will be displayed in the "Network Information" section

### 2. Device Discovery
1. Click "Scan Network" to discover devices on your network
2. Devices will appear in the "Connected Devices" section
3. Each device shows IP addresses, MAC address, and hostname

### 3. Packet Monitoring
1. Click "Start Monitoring" to begin packet capture
2. The application will track packets sent/received by each device
3. Data usage is calculated and displayed in GB
4. Use "Stop Monitoring" to halt packet capture

### 4. Auto-Scan Feature
1. Click "Toggle Auto-scan" to enable automatic device discovery
2. The system will scan for new devices every 30 seconds
3. Useful for detecting new devices joining the network

### 5. Data Management
1. Use "Reset Data" to clear all device statistics
2. Use "Refresh" to update the device list manually
3. Toggle "Show online only" to filter active devices

## Security Considerations

### Important Notes
- This application requires sudo/administrator privileges for packet capture
- Only use on networks you own or have permission to monitor
- The application is designed for local testing and DoS algorithm development
- Do not expose the web interface to the public internet without proper security measures

### Firewall Configuration
If running on Raspberry Pi with external access:
```bash
# Allow access to the application port
sudo ufw allow 5001/tcp
sudo ufw enable
```

## Troubleshooting

### Common Issues

#### 1. Permission Denied Errors
```bash
# Ensure you're running with sudo
sudo python src/main.py
```

#### 2. No Devices Found
- Check network connectivity
- Ensure you're on the correct network interface
- Try running a manual scan multiple times
- Check if devices are actually connected to the network

#### 3. Packet Monitoring Not Working
- Verify sudo privileges
- Check if the network interface supports promiscuous mode
- Some virtual machines may not support packet capture

#### 4. Port Already in Use
```bash
# Check what's using the port
sudo lsof -i :5001
# Kill the process or change the port in src/main.py
```

### Performance Optimization

#### For Raspberry Pi
- Consider using a lightweight web server like Gunicorn for production:
```bash
pip install gunicorn
gunicorn -w 2 -b 0.0.0.0:5001 src.main:app
```

#### Memory Usage
- The application stores device data in memory
- For long-running monitoring, consider implementing data persistence
- Monitor memory usage with large numbers of devices

## API Endpoints

The application provides REST API endpoints for integration:

- `GET /api/network/devices` - Get all devices
- `GET /api/network/devices/online` - Get online devices only
- `GET /api/network/devices/<mac>/stats` - Get specific device stats
- `POST /api/network/scan` - Trigger network scan
- `POST /api/network/monitoring/start` - Start packet monitoring
- `POST /api/network/monitoring/stop` - Stop packet monitoring
- `GET /api/network/monitoring/status` - Get monitoring status
- `POST /api/network/reset` - Reset all data

## Customization

### Changing Network Interface
Edit `src/network_scanner.py` to specify a particular interface:
```python
# In get_network_info() method
return {
    'interface': 'wlan0',  # Specify your interface
    'ip': '192.168.1.1',
    'netmask': '255.255.255.0',
    'network_range': '192.168.1.0/24'
}
```

### Modifying Scan Intervals
Edit `src/routes/network.py`:
```python
# In auto_scan_worker() function
time.sleep(30)  # Change scan interval (seconds)
```

### Customizing Web Interface
- Modify `src/static/styles.css` for appearance changes
- Edit `src/static/script.js` for functionality changes
- Update `src/static/index.html` for layout modifications

## Support and Development

This application is designed for educational and testing purposes. For production use, consider:
- Adding authentication and authorization
- Implementing data persistence (database)
- Adding logging and monitoring
- Implementing rate limiting
- Adding HTTPS support
- Creating systemd service files for automatic startup

## License and Disclaimer

This software is provided for educational and testing purposes only. Users are responsible for ensuring compliance with local laws and regulations regarding network monitoring and packet capture.

