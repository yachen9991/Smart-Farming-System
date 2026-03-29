# Smart Farming System

The system uses two **Raspberry Pi Pico** boards communicating via **UART**. One board collects data from multiple sensors, while the other acts as a Master node to process the data and alert the user if conditions (like temperature or light) are outside the optimal range.

## Sensors & Interfaces
We integrated three different sensors to monitor the plants, using both digital and analog interfaces:

| Sensor | Purpose | Interface |
| :--- | :--- | :--- |
| **BME680** | Temperature & Humidity | **I2C** |
| **VEML7700** | Ambient Light | **I2C** |
| **SEN0114** | Soil Moisture | **ADC** |

## Master board Implementation Details
- **Board-to-Board**: The atwo Picos are connected via **UART** (Pins P8/P9).
- **Data Handling**: We used Zephyr's **Message Queues (`k_msgq`)** to ensure sensor data is sent and received reliably without dropping characters.
- **Alert Logic**: The system automatically flags warnings if the temperature exceeds **30°C** or if light levels are not ideal for plant growth.

## Sensor board Implementation Details
- **I2C Communication**: We implemented **I2C** polling to read raw data from the BME680 and VEML7700 sensors. This involved configuring the I2C peripheral on the Pico and following datasheet formulas to convert values into physical units.
- **Board-to-Board (UART)**: The two Picos are connected via **UART** (Pins P8/P9). We used Zephyr's **Message Queues (`k_msgq`)** to ensure data is sent from the sensor node and received by the master node reliably.
- **Alert Logic**: The system monitors thresholds and flags warnings if:
  - Temperature exceeds **30°C**.
  - Light levels are outside the optimal range (**10,000 - 25,000 Lux**).
  - Soil moisture is too low (Dry) or too high (In Water).
