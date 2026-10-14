# SECURE-GSM---BASED-THERMAL-MONITORING-AND-SET--POINT-CONTROL-SYSTEM
Secure GSM-Based Thermal Monitoring and Set-Point Control System using LPC2148 (ARM7). Monitors temperature and humidity with a DHT11 sensor, stores secure settings in EEPROM via I2C, displays data on an LCD, maintains time using an RTC, and sends SMS alerts through a GSM module when temperature exceeds the configured threshold.

# Objectives
- Monitor temperature and relative humidity in real time.
- Compare sensor readings with predefined set-points.
- Display sensor readings and system status on an LCD.
- Generate fault indications using an LED or buzzer.
- Send SMS alerts to an authorized mobile number.
- Store set-points, passwords, and mobile numbers in EEPROM.
- Support secure remote configuration using SMS commands.
- Allow local set-point and password changes through a keypad.
- Use UART interrupts for GSM communication.
- Include timestamps in fault notifications using the RTC.

# Features

1. **Real-Time Monitoring:** Continuously reads temperature and humidity from the DHT11 sensor.
2. **Set-Point Comparison:** Detects temperature values exceeding the configured limit.
3. **SMS Alerts:** Notifies authorized users when abnormal conditions occur.
4. **Password-Protected Commands:** Validates SMS commands before performing configuration changes.
5. **Remote Set-Point Modification:** Changes the temperature limit through SMS.
6. **Mobile Number Management:** Updates the registered alert recipient through an authenticated command.
7. **Remote Sensor Information:** Sends current sensor readings through SMS.
8. **Non-Volatile Storage:** Preserves configuration data in AT24C256 EEPROM.
9. **Local Configuration:** Uses a keypad and LCD to modify settings.
10. **Failed Login Protection:** Provides fault indication for incorrect passwords and temporary blocking after three consecutive incorrect local attempts.
11. **Interrupt-Based Communication:** Uses UART interrupts for GSM data reception.
12. **Timestamped Alerts:** Uses the RTC to associate fault notifications with the time of occurrence.

# Block diagram
<img width="425" height="405" alt="image" src="https://github.com/user-attachments/assets/e3faff74-4970-4fee-9256-1be33f7543e1" />

# Hardware Requirements:
1.LPC2148.\
2.LCD\
3.AT24C256\
4.DTH11\
5.4X4 MATRIX KEYPAD\
6.SWITCH\
7.LED’S\
8.GSM MODULE (M660A)\
9.DB-9 CABLE/USB-UART CONVERTER

# SOFTWARE REQUIREMENTS:
1.KEIL C Compiler\
2.PROGRAMMING IN EMBEDDED C\
3.Flash Magic.

# System Architecture
The LPC2148 acts as the central controller and coordinates all peripherals.\
- **DHT11 → LPC2148:** Provides temperature and humidity readings.\
- **LPC2148 → LCD:** Displays sensor values and system status.\
- **LPC2148 ↔ AT24C256:** Reads and writes configuration data using I²C.\
- **Keypad → LPC2148:** Supplies local configuration commands.\
- **LPC2148 ↔ GSM module:** Exchanges SMS data through UART.\
- **RTC → LPC2148:** Provides the timestamp for fault notifications.\
- **LPC2148 → LEDs / Buzzer:** Indicates abnormal conditions and authentication failures.

## ⚙️ Working Principle
### Step 1: System Initialization
The LPC2148 initializes the LCD, keypad, UART, I²C, sensor driver, and other required peripherals. The GSM module is initialized using AT commands.

### Step 2: Load Configuration
The controller reads the temperature and humidity set-points, password, and authorized mobile number from predefined EEPROM locations.

### Step 3: Read Sensor Values
The DHT11 measures the current temperature and relative humidity. The values are displayed on the LCD.

### Step 4: Compare with Set-Points
The microcontroller compares the sensor readings with their configured limits.
- If the temperature is within the limit, monitoring continues.
- If the temperature exceeds the limit, the controller activates the fault indication and initiates an SMS alert.
- Humidity monitoring can use the same process when its alarm logic is implemented.

### Step 5: Send SMS Alerts
The GSM module sends a notification to the registered mobile number. The message can contain the current temperature, configured limit, fault status, and RTC timestamp.

### Step 6: Process Remote Commands
When an SMS is received, the controller validates the sender, password, command syntax, and supplied data. Valid commands can update the temperature set-point, change the registered mobile number, or request sensor information.

### Step 7: Local Configuration
An external interrupt activates the LCD menu. The user enters the current password and selects either set-point modification or password modification. Valid changes are stored in EEPROM.

### Step 8: Repeat Monitoring
The controller returns to continuous sensor monitoring and processes new commands as they arrive.

## 📲 SMS Command Format
The command format is:
`XXXXCDDDD...$`
| Field | Meaning |
|---|---|
| `XXXX` | Four-digit access password |
| `T` | Change temperature set-point |
| `M` | Change registered mobile number |
| `I` | Request current sensor information |
| `$` | End-of-command marker |

### 1. Change Temperature Set-Point
Example:
`0786T38$`
The controller validates the command and saves the new temperature set-point, 38°C, to EEPROM.

### 2. Change Registered Mobile Number
Example:
`0786M9866666699$`
The controller validates the command and stores the supplied mobile number in EEPROM.

### 3. Request Sensor Information
Example:
`0786I$`
The controller reads the current sensor values and sends the information to the registered mobile number.
**Security:** The password and mobile number shown above are illustrative examples. Use your own test credentials. Configuration changes should be accepted only from the authorized sender, with proper input validation and additional security measures where possible.

# Flow chart
Secure-GSM-Thermal-Monitoring/
│
├── README.md
├── src/
│   ├── projectmain.c
│   ├── lcd.c
│   ├── lcd.h
│   ├── delay.c
│   ├── delay.h
│   ├── keypad.c
│   ├── keypad.h
│   ├── i2c.c
│   ├── i2c.h
│   ├── uart.c
│   ├── uart.h
│   ├── adc.c
│   └── adc.h
│
├── drivers/
│   └── dht11.c
│
├── docs/
│   ├── block-diagram.png
│   ├── flowchart.png
│   └── circuit-diagram.png
│
├── screenshots/
│   ├── lcd-output.png
│   └── sms-alert.png
│
└── LICENSE


## 📟 Expected Output:


### Normal Condition

Example configuration:

- Temperature set-point: 38°C
- Current temperature: 32°C
- Humidity set-point: 60%
- Current humidity: 55%

The LCD displays the readings, and the controller continues monitoring because the temperature is below the configured limit.

### Abnormal Temperature Condition

If the current temperature rises to 42°C while the limit is 38°C:

- The red LED or buzzer is activated.
- The GSM module sends an SMS alert.
- The notification can include the current temperature, configured limit, and timestamp.
- The system continues monitoring.

Example SMS:

```text
THERMAL ALERT
Temperature: 42 C
Set Point: 38 C
Status: HIGH TEMPERATURE
Time: RTC Timestamp
```

### Remote Configuration

When a valid command is received:

- The temperature set-point is updated.
- The registered mobile number is changed, or sensor information is returned.
- Configuration changes are saved in EEPROM.

These are expected outputs based on the specified program logic. Actual screenshots and measured results should be added after testing the hardware.

## 🌍 Applications

1. **Industrial Machinery:** Monitor environmental temperature and notify maintenance personnel of abnormal conditions.
2. **Pharmaceutical Storage:** Monitor temperature and humidity in suitable storage environments.
3. **Greenhouses:** Track environmental conditions and support ventilation control when actuators are installed.
4. **Telecommunication Rooms:** Monitor equipment-room conditions at remote sites.
5. **Electrical Control Panels:** Detect excessive ambient temperature inside enclosures.
6. **Warehouses:** Monitor environmental conditions in temperature-sensitive storage areas.
7. **Remote Facilities:** Provide SMS-based supervision where a local network or internet connection is unavailable but cellular coverage exists.

The suitability of each application depends on the sensor, measurement range, environmental requirements, and safety provisions.

## 🚀 Future Scope

### 1. IoT and Cloud Integration
Connect the system to an IoT platform for real-time dashboards, remote monitoring, and centralized alerts.

### 2. Mobile Application
Develop an Android application for viewing sensor readings, changing set-points, and managing alerts.

### 3. Industrial-Grade Sensors
Replace the DHT11 with suitable industrial temperature and humidity sensors where improved accuracy or operating range is required.

### 4. Automatic Heating and Cooling
Add properly rated output drivers, relays or contactors, and control logic for automated thermal regulation.

### 5. Improved Security
Implement stronger authentication, command rate-limiting, audit logging, and protection against replayed or unauthorized commands.

### 6. Data Logging
Store sensor readings and fault history for later analysis and maintenance.

### 7. Backup Power
Add battery backup and power-failure notification to improve monitoring availability.

### 8. Multi-Zone Monitoring
Extend the system to support multiple sensors and separate set-points for several machines or rooms.

### 9. Predictive Maintenance
Analyze recorded trends to identify recurring abnormal conditions and potentially detect early signs of equipment problems.

### 10. Independent Safety Protection
Add appropriate independent over-temperature protection, watchdog monitoring, and sensor-failure handling for safety-critical installations.

## ⚠️ Limitations

- SMS delivery depends on cellular coverage and may be delayed.
- The DHT11 has limited accuracy and operating range for demanding industrial applications.
- A four-digit password provides only basic security.
- EEPROM data requires proper validation and write-management logic.
- The described hardware list does not explicitly include heater/cooler actuator drivers.
- A GSM alert should not be the sole safety mechanism for critical industrial equipment.
- RTC integration and the actual circuit connections must be verified against the implementation.

## 🏁 Conclusion

The Secure GSM-Based Thermal Monitoring and Set-Point Control System demonstrates embedded monitoring, sensor interfacing, EEPROM data storage, UART interrupt handling, GSM communication, and password-protected remote configuration.

It enables users to monitor environmental conditions, receive fault notifications, and modify settings without being physically present at the installation. With industrial-grade sensors, reliable actuator control, stronger security, and IoT integration, the design can be extended into a more capable industrial monitoring and control system.

## 🔑 Keywords

`LPC2148` `Embedded C` `GSM` `SMS Alert` `DHT11` `Temperature Monitoring` `Humidity Monitoring` `EEPROM` `UART Interrupt` `I2C` `Keil` `Flash Magic` `Industrial Automation` `Embedded Systems`

