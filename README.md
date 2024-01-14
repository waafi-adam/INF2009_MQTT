## **IoT Communications: Setting Up MQTT Broker, Publisher, and Subscriber on Raspberry Pi 400**

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

**Introduction:**
MQTT stands for Message Queue Telemetry Transport. It is a publish/subscribe messaging transport protocol that is lightweight, open, and designed to be easy to implement. These characteristics make it ideal for use in many situations, including constrained environments such as for communication in the Internet of Things (IoT) contexts where a small code footprint is required and network bandwidth is scarce. The protocol runs over TCP/IP, or over other similar network protocols that provide ordered, lossless, bidirectional connections.

MQTT can be broken down into the following components:
**MQTT Broker:** The broker accepts messages from clients (publishers) and then delivers them to any interested clients (subscribers). Each message must belong to a specific topic. The broker is a program running on a device that acts as an intermediary between clients who publish messages and clients who have made subscriptions. 

**Topic:** A namespace (or place) for messages on the broker. Clients must subscribe to and/or publish to a topic.

**MQTT Client:** The client is a ‘device’ that either publishes a message to a specific topic or subscribes to a topic, or both. A publisher is a client that sends a message to the broker, using a topic name. While a subscriber informs the broker which topics it is interested in. Once subscribed, the broker sends messages published to that topic. A client can subscribe to multiple topics. However, a client must first establish a connection with the broker and can take the following actions:
•    Publish messages that other clients might be interested in (subscribed to).
•    Subscribe to a specific topic to receive messages from the publishers.
•    Unsubscribe to remove a request for the messages.
•    Disconnect from the Broker.

![image](https://github.com/drfuzzi/INF2009_IoTComms/assets/108112390/cfa167ee-d747-45ee-be2f-90c795415767)
Figure 1: An example of MQTT implementation

**Lab Exercise:**

**1. Install and Configure the MQTT Broker on a Raspberry Pi 400:**

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

**2. Install and Configure the MQTT Publisher and Subscriber on another Raspberry Pi 400:**

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

**3. Testing your MQTT Communication:**

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
