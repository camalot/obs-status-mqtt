
![](https://i.imgur.com/lwiYgXK.png)

## TOPICS

- `stream/obs/recording/availability`
- `stream/obs/streaming/availability`
- `stream/obs/recording/sensor`
- `stream/obs/streaming/sensor`
- `stream/obs/camera`

## FLOW VARIABLES

Replace the following in the `obs-status.json` with the values for your setup. You may have to configure the MQTT Broker / OBS Connections in node-red separately.

- `${MQTT_BROKER}` : Hostname/IP Address for your MQTT Broker
- `${OBS_HOST}` : Hostname/IP Address for your OBS machine
- `${OBS_MAIN_SCENE}` : The OBS Scene Name that you want to use to get the screen capture for.

In the `Output Payload` you can change the `blank` to be a base64 encoded value of the image you want to display when you are not recording/streaming.

## MQTT SENSORS IN HOME ASSISTANT

```yaml
camera:
- name: "OBS Streaming Preview"
  topic: "stream/obs/camera"
  unique_id: "obs_camera_preview"
  encoding: ""
  image_encoding: "b64"
  availability:
    topic: "stream/obs/streaming/availability"
    value_template: "{{ value_json.state }}"
    payload_not_available: "offline"
    payload_available: "online"
  device:
    manufacturer: OBS
    model: OBS Websockets
    sw_version: v5.x
    identifiers:
    - ws://192.168.1.111:4455

binary_sensor:
- name: "OBS Streaming"
  state_topic: "stream/obs/streaming/sensor"
  unique_id: "obs_streaming_sensor"
  payload_on: true
  payload_off: false
  value_template: "{{ value_json.state }}"
  availability:
    topic: "stream/obs/streaming/availability"
    value_template: "{{ value_json.state }}"
    payload_not_available: "offline"
    payload_available: "online"
  device:
    manufacturer: OBS
    model: OBS Websockets
    sw_version: v5.x
    identifiers:
    - ws://192.168.1.111:4455
  qos: 0

- name: "OBS Recording"
  state_topic: "stream/obs/recording/sensor"
  unique_id: "obs_recording_sensor"
  payload_on: true
  payload_off: false
  value_template: "{{ value_json.state }}"
  availability:
    topic: "stream/obs/recording/availability"
    value_template: "{{ value_json.state }}"
    payload_not_available: "offline"
    payload_available: "online"
  device:
    manufacturer: OBS
    model: OBS Websockets
    sw_version: v5.x
    identifiers:
    - ws://192.168.1.111:4455
  qos: 0
```

### OBS OPEN BUT NOT STREAMING/RECORDING  
![](https://i.imgur.com/hBlDXd1.png)

### OBS CLOSED  
![](https://i.imgur.com/YTfj4rv.png)

### HOME ASSISTANT DEVICE INFO
![](https://i.imgur.com/mZysmYU.png)