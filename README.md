# Home Assistant MQTT Assignment – Nitesh Billa

**Student Name:** Nitesh Billa  
**Register Number:** 42130583  
**College:** Sathyabama Institute of Science and Technology  
**Course:** B.E – ECE, 7th Semester  

---

## Objective

To publish temperature, humidity, and light level values from a Python script to an MQTT broker and display them as sensors on a Home Assistant dashboard.

---

## System Components

- Home Assistant OS (VirtualBox)
- Mosquitto MQTT Broker (Home Assistant add-on)
- MQTT Integration in Home Assistant
- Python 3.x
- `paho-mqtt` Python library
- Windows 11 host machine

---

## MQTT Details

- **Broker IP:** `10.55.165.79`
- **Port:** `1883`
- **Username:** `nitesh`
- **Password:** `naninani`
- **Topic:** `home/nitesh-2025/sensor`
- **Payload:** `temp=25,humidity=60,light=80`

---

## Python Script

File: `nitesh_mqtt_publisher.py` (or `python.py`)

```python
import time
import paho.mqtt.client as mqtt

student_name = "Nitesh"
unique_id = "42130583"

CLIENT_ID = "client_" + unique_id
BROKER_IP = "10.55.165.79"
BROKER_PORT = 1883

MQTT_USERNAME = "nitesh"
MQTT_PASSWORD = "naninani"

topic = "home/nitesh-2025/sensor"

def on_connect(client, userdata, flags, rc, properties=None):
    print("Connected" if rc == 0 else f"Failed to connect: {rc}")

client = mqtt.Client(mqtt.CallbackAPIVersion.VERSION2, client_id=CLIENT_ID)
client.username_pw_set(MQTT_USERNAME, MQTT_PASSWORD)
client.on_connect = on_connect

print(f"Connecting to {BROKER_IP}:{BROKER_PORT} as {CLIENT_ID}...")
client.connect(BROKER_IP, BROKER_PORT, keepalive=60)
client.loop_start()

try:
    publish_count = 0
    max_publishes = 5

    while publish_count < max_publishes:
        message = "temp=25,humidity=60,light=80"
        client.publish(topic, message, qos=1, retain=True)
        print("Published →", message)
        publish_count += 1
        time.sleep(5)

except KeyboardInterrupt:
    print("Stopped")

finally:
    client.loop_stop()
    client.disconnect()
