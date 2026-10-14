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

## 🛠️ Hardware Requirements

 Component                              Purpose 
 
| LPC2148 microcontroller|              |Main processing and control unit |
| DHT11 sensor                         Temperature and humidity measurement |
| LCD |                                Displays sensor readings, menus, and status |
| AT24C256 EEPROM                      Stores set-points, password, and mobile number |
| 4×4 matrix keypad                    Local user input and configuration |
| GSM module (M660A)                    SMS transmission and reception |
| LEDs                                  Normal and fault indications |
| Buzzer (optional)                     Audible fault indication |
| Switch / external interrupt input      Activates local configuration |
| DB-9 cable / USB-UART converter        Serial communication and module testing |
| RTC                                    Provides timestamps for fault alerts |
| Power supply                             Powers the controller and peripheral modules |
