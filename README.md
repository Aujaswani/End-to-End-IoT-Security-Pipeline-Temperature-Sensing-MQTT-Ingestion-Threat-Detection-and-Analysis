# End-to-End-IoT-Security-Pipeline-Temperature-Sensing-MQTT-Ingestion-Threat-Detection-and-Analysis


This project implements a complete IoT security pipeline that collects physical environmental data, transmits it through MQTT, detects network threats using an IDS, and visualizes system activity using the ELK Stack. The solution simulates real-world IoT deployments commonly found in industrial cybersecurity environments.

---

##  Objectives
- Capture real-time temperature data from a DS18B20 sensor via Raspberry Pi.
- Publish edge telemetry to a cloud-hosted MQTT broker.
- Detect malicious network behavior using Suricata IDS.
- Ship event logs for centralized analysis using Filebeat.
- Build Kibana dashboards that visualize temperature telemetry and security alerts.

---

##  Hardware Components
![image](https://github.com/user-attachments/assets/001a886b-96a7-42a3-906a-8b249236aafd)
| Component | Description |
|----------|-------------|
| Raspberry Pi | Edge compute device handling sensor ingestion & publishing |
| Breadboard | Rapid prototyping base for wiring |
| DS18B20 Waterproof Sensor | Digital 1-Wire temperature probe for real-time readings |



---

## Sensor Wiring

| Wire | Function | Raspberry Pi Pin |
|------|----------|------------------|
| Red | VCC (3.3V) | Pin 1 |
| Black | Ground | Pin 6 |
| Yellow | Data (1-Wire) | GPIO4 — Pin 7 |

---

##  Edge Software Logic (Raspberry Pi)
- 1-Wire interface enabled for hardware communication
- Python script continuously reads sensor output
- Readings are converted to Celsius
- Values are serialized into JSON payloads
- JSON is published to MQTT topic hosted by cloud EC2

---

##  Cloud Broker Layer (AWS EC2)
- Mosquitto broker receives all incoming MQTT telemetry
- `mqtt_logger.py` subscribes to the topic
- All readings are written into `temp.log` for ingestion
- Traffic is visible from multiple network sources for correlation

---

##  Intrusion Detection (Suricata IDS)
Suricata monitors interface traffic to:

<img width="452" height="298" alt="image" src="https://github.com/user-attachments/assets/e20028c2-b89a-4d77-ae7b-1c0daced7827" />

- Detect abnormal payload patterns
- Flag malformed JSON injection attempts
- Trigger alerts via custom Lua rules
- Detect lightweight DDoS simulations
- Export structured security events in `eve.json`

Security events include:
- Source/Destination IP
- Event category
- Severity
- Timestamp

---

##  Log Shipping Pipeline (Filebeat)
Filebeat forwards:
- Suricata alerts (`eve.json`)
- Temperature telemetry (`temp.log`)
- <img width="452" height="220" alt="image" src="https://github.com/user-attachments/assets/64b2162d-8ed0-41a4-afb5-d25be61f3115" />


Features:
- JSON parsing for IDS alerts
- Field mapping consistency
- Automatic ingestion into Elasticsearch indices

---

##  ELK Stack Visualization
Kibana dashboards visualize:
- Temperature trends over time
- Source IPs of detected threats
- Alert categories and frequency
- Timeline correlation between attacks and sensor activity
- System health metrics
<img width="468" height="305" alt="image" src="https://github.com/user-attachments/assets/53ab46ad-b4ee-424b-98cc-8f6e7bd5901f" />


Dashboards support:
- Filtering by timestamp
- Drill-down investigations
- Event correlation patterns

---

##  Attack Simulation
Attacks performed to validate detection pipeline:
- Synthetic DDoS-style bursts
- Malformed JSON payload injection
- Custom Suricata rule activation

Suricata successfully:
- Logged anomalies
- Categorized alerts
- Exported JSON-formatted events to ELK

---

##  Correlation Capabilities
The final pipeline allows:
- Linking sensor anomalies to network alerts
- Detecting attacker intent through payload structure
- Monitoring compromised IoT telemetry streams
- Observing operational behavior alongside threat activity

---

##  Troubleshooting Highlights
Challenges encountered included:
- Security group port restrictions
- YAML indentation errors in Suricata config
- Kibana data view mapping delays

Solutions involved:
- Adjusting inbound rules
- Correcting configuration structure
- Refreshing index patterns

---

## Skills Demonstrated
- IoT sensor data acquisition
- Cloud MQTT ingestion
- Network intrusion detection
- Log shipping + parsing
- Full ELK visualization stack
- Alert correlation & analysis
- Real-world security pipeline integration

---

##  Future Improvements
Potential enhancements:
- TLS & authentication on MQTT
- Email/SMS alerting
- Multiple environmental sensors
- ML-based anomaly scoring
- Automated pipeline enrichment via Logstash grok filters

---

## Conclusion
This end-to-end IoT security pipeline demonstrates how physical sensor telemetry, network security monitoring, and cloud analytics can be combined into a cohesive cyber-defense system. By integrating hardware, protocols, and security tooling, we gained hands-on experience building and analyzing a modern IoT threat detection environment.

---
