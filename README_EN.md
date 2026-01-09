# Serial Port Debug Assistant User Guide

## Application Overview

Serial Port Debug Assistant is a serial communication tool developed by the SUNTEK team for Android devices, supporting communication and debugging with LED controllers and barcode scanners through serial ports.

## Port Description

This application supports two serial port devices:

- **ttyS5** - LED Controller Serial Port
  - Used for controlling LED display, brightness adjustment, and other operations
  - Baud rate: 9600

- **ttyS11** - Barcode Scanner Serial Port
  - Used for receiving scan data from barcode scanners
  - Baud rate: 9600

## Features

### 1. Port Selection
- Supports selection of `/dev/ttyS5` or `/dev/ttyS11`
- Automatically detects port availability, displaying "Available" or "Unavailable" status
- Drop-down list selection for easy operation

### 2. Baud Rate Setting
- Default baud rate: 9600
- Baud rate can be modified according to device requirements
- Supports standard baud rates: 9600, 19200, 38400, 57600, 115200

### 3. Serial Port Control
- **Open Serial Port**: Click the "Open Serial Port" button to connect to the selected port
- **Close Serial Port**: After opening, the button changes to "Close Serial Port". Click to disconnect
- After opening the serial port, port selection and baud rate settings will be locked to prevent misoperation

### 4. Data Reception
- Real-time serial port data reception with timestamp display
- Display formats:
  - Text mode: Shows printable characters, non-printable characters displayed in hexadecimal format `[XX]`
  - Each data entry displays reception time: `[HH:mm:ss.SSS]`
- Auto-scroll to latest data
- Supports long-press to select and copy received data

### 5. Data Transmission
- **Text Mode**: Directly input text content to send
- **Hex Mode**: Click the "Hex Send" button to switch to hexadecimal transmission mode
  - Input hexadecimal data, such as: `01 02 03 FF` or `010203FF`
  - Automatically filters spaces
  - Hex mode will also display data in hexadecimal format in the receive area
- Each sent data will be displayed in the receive area with a timestamp
- Displays the number of bytes sent after successful transmission

### 6. Other Features
- **Clear Receive**: Clear all data in the receive area
- **Status Display**: Bottom displays current connection status and error messages
- **Auto Check**: Automatically checks if port devices exist on startup

## Usage Instructions

### Connecting to LED Controller (ttyS5)

1. **Select Port**
   - Click the port dropdown list
   - Select `/dev/ttyS5 (Available)` or `/dev/ttyS5 (Unavailable)`
   - If it shows "Unavailable", check device permissions

2. **Set Baud Rate**
   - Confirm the baud rate is `9600` (default value)
   - If modification is needed, input the corresponding baud rate value

3. **Open Serial Port**
   - Click the "Open Serial Port" button
   - Upon success, it will display: "Connected: /dev/ttyS5 @ 9600 baud"
   - Status bar shows "Connected" status

4. **Send LED Control Commands**
   - Input LED control commands in the send area (refer to LED communication protocol documentation)
   - Select "Text Send" or "Hex Send" according to command format
   - Click the "Send Data" button

5. **Receive Response Data**
   - Data returned by the LED controller will be displayed in the receive area
   - Each data entry has a timestamp
   - You can view the response status of the LED controller

6. **Close Serial Port**
   - After debugging is complete, click the "Close Serial Port" button
   - Disconnect the serial port connection

### Connecting to Barcode Scanner (ttyS11)

1. **Select Port**
   - Click the port dropdown list
   - Select `/dev/ttyS11 (Available)`

2. **Set Baud Rate**
   - Confirm the baud rate is `9600`

3. **Open Serial Port**
   - Click the "Open Serial Port" button
   - Wait for connection success prompt

4. **Receive Scan Data**
   - After the serial port is opened, barcode/QR code data scanned by the scanner will automatically display in the receive area
   - Each data entry has a timestamp: `[HH:mm:ss.SSS] RX: Scan content`
   - You can view scan results in real-time

5. **Send Configuration Commands** (if needed)
   - If the scanner needs to be configured, input configuration commands in the send area
   - Click "Send Data" to send

6. **Close Serial Port**
   - After use, click "Close Serial Port" to disconnect

## Data Format Description

### Text Mode
- Directly input text content, such as: `AT+CONFIG=1`
- Suitable for sending ASCII text commands

### Hex Mode
- Input hexadecimal data, format examples:
  - `01 02 03 FF` (with spaces)
  - `010203FF` (without spaces)
  - `0A 0D` (carriage return and line feed)
- Automatically filters spaces and case
- Suitable for sending binary data or protocol data packets

### Received Data Display
- **Text Mode**: Printable characters display normally, non-printable characters displayed in `[XX]` format
- **Hex Mode**: All data displayed in hexadecimal, such as: `01 02 03 FF`

## Interface Description

```
┌─────────────────────────┐
│   Serial Port Debug     │
│      Assistant          │
├─────────────────────────┤
│ Port: [ttyS5 ▼]        │
│ Baud Rate: [9600]       │
│ [Open Serial Port]      │
├─────────────────────────┤
│ Receive Area:           │
│ ┌─────────────────────┐ │
│ │ [10:23:45.123] RX:  │ │
│ │ Data content...     │ │
│ └─────────────────────┘ │
│ [Clear] [Hex Send]      │
├─────────────────────────┤
│ Send Area:              │
│ ┌─────────────────────┐ │
│ │ Input data to send  │ │
│ └─────────────────────┘ │
│ [Send Data]             │
├─────────────────────────┤
│ Status: Connected       │
└─────────────────────────┘
```

## Important Notes

### Permission Requirements
- Access to `/dev/ttyS*` devices requires appropriate system permissions
- If the application cannot open the serial port, you may need:
  1. **Root Permission**: Run on a rooted device
  2. **System Permission**: Device manufacturer has authorized application to access serial ports
  3. **SELinux Policy**: SELinux policy needs to be adjusted to allow access to serial port devices

### Serial Port Usage Rules
1. **Exclusive Access**: Only one application can access the serial port at a time
2. **Close Connection**: Always close the serial port after use to release resources
3. **Exception Handling**: If connection is abnormal, close the serial port first, then reopen

### Baud Rate Setting
- Baud rate must match the device, otherwise normal communication is impossible
- LED controller and barcode scanner default to **9600** baud rate
- After modifying the baud rate, close and reopen the serial port for changes to take effect

### Data Transmission
- Ensure the serial port is opened before sending data
- In Hex mode, ensure the input hexadecimal data format is correct
- If transmission fails, check the serial port connection status

### Data Reception
- Receive area shows maximum visible content, regular clearing is recommended
- Received data will automatically add timestamps for easy debugging
- Long-press text in receive area to select and copy

## Frequently Asked Questions

### Q1: Port shows "Unavailable", unable to open serial port
**A:** 
- Check if device files exist: `/dev/ttyS5` or `/dev/ttyS11`
- Check if the application has access permissions
- Try running the application with root permissions

### Q2: Opening serial port prompts "Failed to open serial port"
**A:**
- Check if the port is occupied by another application
- Check device permission settings
- Try restarting the device and opening again

### Q3: No response after sending data
**A:**
- Check if baud rate matches the device
- Confirm the sent data format is correct
- Check if the device is working normally
- View receive area for any error messages

### Q4: Incomplete barcode scanner data reception
**A:**
- Check baud rate setting (should be 9600)
- Confirm serial port connection is normal
- Check if scanner configuration is correct

### Q5: Hex mode transmission failed
**A:**
- Check if the input hexadecimal data format is correct
- Ensure every two characters represent one byte (00-FF)
- Check for illegal characters

### Q6: Application crashes or unresponsive
**A:**
- Close serial port before exiting the application
- Clear application data and reinstall
- Check Android system logs (logcat) for error information

## LED Controller Usage Examples

### Example 1: Query LED Status
1. Select port: `ttyS5`
2. Open serial port
3. Input query command in send area (refer to LED communication protocol)
4. Click "Send Data"
5. View returned status information in receive area

### Example 2: Control LED Display
1. Open `ttyS5` serial port
2. Switch to Hex mode (click "Hex Send")
3. Input LED control command hexadecimal data
4. Send data, LED displays corresponding content

## Barcode Scanner Usage Examples

### Example 1: Receive Scan Data
1. Select port: `ttyS11`
2. Open serial port
3. Use scanner to scan barcode/QR code
4. Scan results automatically display in receive area with timestamp

### Example 2: Configure Scanner
1. Open `ttyS11` serial port
2. Send configuration commands (refer to scanner protocol documentation)
3. Receive configuration response, confirm if configuration is successful

## Technical Support

If you encounter problems or need technical support, please:
1. Check error messages in the receive area
2. Check prompt messages in the status bar
3. View Android system logs (logcat)
4. Contact SUNTEK technical support team

## Version Information

- Application Name: Serial Port Debug Assistant
- Version: 1.0
- Update Date: 2026.1.9

---

**Note**: Before using serial port communication, please ensure you understand the communication protocols of the LED controller and barcode scanner. Refer to relevant technical documentation.

