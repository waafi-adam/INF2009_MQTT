## IoT Communications: Setting Up MQTT Broker, Publisher, and Subscriber on Raspberry Pi 400**

**Objective:** To learn how to install and configure an MQTT broker, create MQTT publisher and subscriber clients, and test communication between them on a Raspberry Pi 400.

**Prerequisites:**
1. Raspberry Pi 400 with an operating system (e.g., Raspbian) already set up.
2. Internet connection for the Raspberry Pi.

**Materials:**
1. Raspberry Pi 400.
2. Power supply for Raspberry Pi.
3. Keyboard, mouse, and monitor for Raspberry Pi (if not using SSH).
4. MQTT broker (e.g., Mosquitto).
5. MQTT publisher and subscriber clients (e.g., Python Paho MQTT library).

**Lab Steps:**

**1. Install and Configure the MQTT Broker (30 minutes):**

   a. Update the Raspberry Pi's package list:
   ```
   sudo apt update
   ```

   b. Install the Mosquitto MQTT broker:
   ```
   sudo apt install mosquitto
   ```

   c. Start and enable Mosquitto to run on boot:
   ```
   sudo systemctl start mosquitto
   sudo systemctl enable mosquitto
   ```

   d. Verify that Mosquitto is running:
   ```
   systemctl status mosquitto
   ```

**2. Create MQTT Publisher and Subscriber (30 minutes):**

   a. Install the Python Paho MQTT library for both publisher and subscriber:
   ```
   pip install paho-mqtt
   ```

   b. Create a Python script for the MQTT publisher (`mqtt_publisher.py`):
   ```python
   import paho.mqtt.client as mqtt
   import time

   client = mqtt.Client("Publisher")
   client.connect("localhost", 1883)

   while True:
       client.publish("test/topic", "Hello, MQTT!")
       time.sleep(5)
   ```

   c. Create a Python script for the MQTT subscriber (`mqtt_subscriber.py`):
   ```python
   import paho.mqtt.client as mqtt

   def on_message(client, userdata, message):
       print(f"Received message '{message.payload.decode()}' on topic '{message.topic}'")

   client = mqtt.Client("Subscriber")
   client.on_message = on_message
   client.connect("localhost", 1883)
   client.subscribe("test/topic")
   client.loop_forever()
   ```

**3. Test MQTT Communication (30 minutes):**

   a. Open two terminal windows on the Raspberry Pi.

   b. In the first terminal, run the MQTT subscriber:
   ```
   python mqtt_subscriber.py
   ```

   c. In the second terminal, run the MQTT publisher:
   ```
   python mqtt_publisher.py
   ```

   d. Observe the messages being published and received in the subscriber terminal.

**4. Optional Enhancements (30 minutes):**

   a. Secure your MQTT broker by setting up authentication and encryption.

   b. Experiment with different MQTT topics and payloads in your publisher and subscriber.

**5. Conclusion and Cleanup (10 minutes):**

   a. Discuss MQTT and its applications.

   b. Stop the MQTT publisher and subscriber (Ctrl+C).

   c. Disable and stop the Mosquitto MQTT broker:
   ```
   sudo systemctl disable mosquitto
   sudo systemctl stop mosquitto
   ```

**Assessment:**
- Demonstrate successful communication between the MQTT publisher and subscriber.
- Explain how to set up authentication and encryption for MQTT communication.

**Note:** The times provided are approximate and may vary based on individual proficiency with Raspberry Pi and MQTT.
