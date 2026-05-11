# Off-grid IoT Field Monitor

This project provides a robust, solar-powered, and completely solder-less monitoring solution using the Raspberry Pi Pico 2W. It was designed to survive the seasonal gales and humidity of Hayama, Japan, functioning as a "digital guardian" for aquaponics and garden environments.

**Key Features**
 - Immortal Power Solution: Optimized for off-grid use. Calculated to survive ~6 days on a 3,000mAh battery with only 5 hours of sunlight required for a full recharge.
 - High-Side Switching: Eliminates "Ghost" current leaks in RGB LEDs to maximize energy efficiency.
 - Watchdog Resilience: Custom implementation to prevent system freezes during TCP/Wi-Fi instabilities.
 - Solder-less Architecture: Uses Ethernet breakout boards, heat-shrink tubing, and repurposed PC parts for professional-grade reliability without a soldering iron.

**Hardware Components**
 - Microcontroller: Raspberry Pi Pico 2W
 - Power: 3.5W Solar Panel + Legacy Mobile Battery (Pass-through supported)
 - Sensors: Capacitive Soil Moisture Sensor
 - Bridge: Ethernet Cable (RJ45) with Breakout Boards
 - Enclosure: Custom waterproofed repurposed household plastics for "Haniwa" clay figure

**Software Architecture**
The code is written in C++ using the Raspberry Pi Pico SDK. It features a modular design:
 - haniwa_main.cpp: Central logic and power management.
 - haniwa_monitor.cpp: Sensor data acquisition and LED signaling (Active Low / High-Side Switching).
 - haniwa_connector.cpp: Wi-Fi connectivity and communication with the HomeServer.

**Power Saving Configuration**

``` C++
// CYW43 Power Management for "Doze" mode (~20mA)
cyw43_wifi_pm(&cyw43_state, cyw43_pm_value(CYW43_PM2_POWERSAVE_MODE, 200, 1, 1, 5));
```

**How to Build**
 - Install the Raspberry Pi Pico SDK.
 - Set up your environment (VS Code + CMake).
 - Clone this repository.
 - Configure your WIFI_SSID and WIFI_PASSWORD in the configuration file.
 - Build and flash the .uf2 file.

**For More Details**

For the full story behind this project—including the "Solder-less" philosophy and the memory of the poplar tree—please visit the official project page:
[https://www.hackster.io/shinji_kameda/off-grid-waterproof-protection-for-field-prototype-tests-851e1a]