# MotionDrive 32  
Gesture Controlled ESP32 Car (MPU6500 + ESP-NOW)

MotionDrive 32 is a wireless gesture-controlled robotic car built using two ESP32 boards, an MPU6500 accelerometer, and an L298N motor driver. The transmitter reads hand tilt and sends directional commands to the receiver using ESP-NOW, allowing low-latency control without WiFi.

------------------------------------------------------------

## Features
- Gesture control: forward, backward, left, right
- Fast, low-latency ESP-NOW communication
- Single battery powers the entire car (ESP32 + L298N + motors)
- 4-motor BO chassis drive system
- Speed control using PWM
- Clean and modular codebase

------------------------------------------------------------

## Hardware Used
- 2 × ESP32 Dev Boards
- MPU6500 or MPU6050 Accelerometer
- L298N Motor Driver
- 4 × BO Motors
- 7.4–12V Battery
- Car chassis
- Jumper wires

------------------------------------------------------------

# Connections

## 1. Transmitter (ESP32 + MPU6500)
MPU6500 VCC  → ESP32 3.3V  
MPU6500 GND  → ESP32 GND  
MPU6500 SDA  → ESP32 GPIO 21  
MPU6500 SCL  → ESP32 GPIO 22  
MPU6500 AD0  → GND  
Transmitter ESP32 powered via USB or LiPo.

------------------------------------------------------------

## 2. Receiver (ESP32 + L298N + Motors)

### ESP32 → L298N (control pins)
IN1 → GPIO 23  
IN2 → GPIO 22  
IN3 → GPIO 19  
IN4 → GPIO 18  
ENA → GPIO 25  
ENB → GPIO 26  

### Motor wiring (4 BO motors)
Left motors (M1, M2):  
Left motors + → L298N OUT1  
Left motors - → L298N OUT2  

Right motors (M3, M4):  
Right motors + → L298N OUT3  
Right motors - → L298N OUT4  

### Power wiring (single battery setup)
Battery +     → L298N +12V  
Battery -     → L298N GND  
L298N 5V      → ESP32 5V  
L298N GND     → ESP32 GND (common ground required)  

Important notes:  
- Do not connect L298N 5V to ESP32 3.3V.  
- ESP32 must share ground with L298N.

------------------------------------------------------------

## ESP-NOW MAC Pairing
The car (receiver) ESP32 prints its MAC address in Serial Monitor:
CAR ESP MAC: xx:xx:xx:xx:xx:xx

Use this MAC address inside the transmitter code:
uint8_t receiverMac[] = { 0x__, 0x__, 0x__, 0x__, 0x__, 0x__ };

------------------------------------------------------------

# How MotionDrive 32 Works
1. The transmitter ESP32 reads accelerometer values (AX, AY) from the MPU6500.  
2. Hand tilt is converted into motion commands:  
   F = Forward  
   B = Backward  
   L = Left  
   R = Right  
   S = Stop  
3. These commands are sent over ESP-NOW to the receiver ESP32.  
4. The receiver ESP32 drives the motors using the L298N motor driver.  
5. Speed is controlled using PWM through ENA and ENB pins.

------------------------------------------------------------

# Code Structure
/motiondrive32_transmitter  
    transmitter.ino  (reads MPU, sends ESP-NOW commands)

/motiondrive32_receiver  
    receiver.ino     (receives commands, drives motors)

------------------------------------------------------------

# Power Notes
- The car runs entirely from one battery connected to the L298N +12V input.  
- L298N's onboard 5V regulator powers the ESP32 via its 5V pin.  
- The transmitter ESP32 is powered separately using USB or a LiPo pack.

------------------------------------------------------------

# License
MIT License.
