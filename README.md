![image](PLACEHOLDER FOR COVER IMAGE)

# 💡 My Experience Improving Light Automations in Home Assistant (🚧UNDER CONSTRUCTION🚧)
_A journey from basic motion triggers to complex context-aware lighting powered by Home Assistant.  
Covers hardware iterations, automation logic, sensor layering, reliability tuning, and user control strategies._

---

## 📚 Table of Contents

1. [🔍 Introduction](#-1-introduction)  
2. [🔌 My First Smart Switches (Tuya Wi-Fi Relays)](#-2-my-first-smart-switches-tuya-wi-fi-relays)  
3. [🏃 Motion Sensors & The AJAX IR Hack](#-3-motion-sensors--the-ajax-ir-hack)  
4. [🌩️ Dealing with Wi-Fi Unreliability](#-4-dealing-with-wi-fi-unreliability)  
5. [☁️ Cloud Lag vs. Local Control](#-5-cloud-lag-vs-local-control)  
6. [👁️ The Power of mmWave Presence Sensors](#-6-the-power-of-mmwave-presence-sensors)  
7. [🧠 Making Automations Smarter with Context ID](#-7-making-automations-smarter-with-context-id)  
8. [🌞 Adding Natural Light Logic with LUX Sensors](#-8-adding-natural-light-logic-with-lux-sensors)  
9. [🧲 Smart Switches Behind Legacy Switches](#-9-smart-switches-behind-legacy-switches)  
10. [🗺️ Mapping Light Entities on Floorplans](#-10-mapping-light-entities-on-floorplans)  
11. [🧩 Final Automation Architecture & Examples](#-11-final-automation-architecture--examples)  
12. [🎬 Live Video Demonstration](#-12-live-video-demonstration)  
13. [🧠 Lessons Learned & Next-Level Ideas](#-13-lessons-learned--next-level-ideas)  
14. [🪪 License](#-14-license)  
15. [👨‍💻 Author and Related Projects](#-15-author-and-related-projects)  

---

## 🔍 1. Introduction [↑](#-table-of-contents)

- Why I started automating lighting  
- The frustrations with dumb switches and “dumb” motion automation  
- What the article will cover

---

## 🔌 2. My First Smart Switches (Tuya Wi-Fi Relays) [↑](#-table-of-contents)

- Aliexpress 1/2-gang Wi-Fi wall switches  
- Initial setup and integration via Tuya Cloud  
- What worked, what didn’t  
- Impact of power outages and reboots

![image](PLACEHOLDER FOR PHOTOS OF FIRST SMART SWITCHES)

---

## 🏃 3. Motion Sensors & The AJAX IR Hack [↑](#-table-of-contents)

- Simple PIR sensors and why they’re not enough  
- Using AJAX MotionProtect as triggers  
- Overview of how the hack works (link to full article)  
- First usable automations and their limitations

![image](PLACEHOLDER FOR AJAX SENSOR + NODE-RED / YAML SNIPPETS)

---

## 🌩️ 4. Dealing with Wi-Fi Unreliability [↑](#-table-of-contents)

- Devices going offline  
- Router limitations (TP-Link case)  
- Migrating DHCP to HA (Dnsmasq)  
- Static DHCP leases, port isolation, etc.

![image](PLACEHOLDER FOR NETWORK MAP / HA DNSMASQ SCREENSHOTS)

---

## ☁️ 5. Cloud Lag vs. Local Control [↑](#-table-of-contents)

- TUYA Cloud integration and its lag  
- When I moved automations into the SmartLife app  
- Discovering LocalTuya and migrating to local control  
- Real improvements in automation speed

```yaml
# Example: LocalTuya light definition in configuration.yaml
localtuya:
  - host: 192.168.1.44
    device_id: YOUR_DEVICE_ID
    local_key: YOUR_LOCAL_KEY
    friendly_name: Hallway Light
```

---

## 👁️ 6. The Power of mmWave Presence Sensors [↑](#-table-of-contents)

- What is mmWave and how it beats PIR  
- My first TUYA Zigbee mmWave sensors  
- Setup with HA Zigbee Coordinator  
- Presence vs motion vs timeouts

![image](PLACEHOLDER FOR SENSOR PHOTOS + PRESENCE ENTITY SCREENSHOTS)

---

## 🧠 7. Making Automations Smarter with Context ID [↑](#-table-of-contents)

- Problem: Automation overrides manual actions  
- Using `trigger.context.id` and `last_changed_by` logic  
- Conditional flows based on interaction source  
- NFC tag to disable automation temporarily

```yaml
# Example: Check if light was turned on by automation
condition:
  - condition: template
    value_template: "{{ trigger.context.user_id == None }}"
```

---

## 🌞 8. Adding Natural Light Logic with LUX Sensors [↑](#-table-of-contents)

- Using brightness to avoid turning on lights during daylight  
- Which LUX sensors I use  
- Threshold calibration per room  
- Avoiding bounce loops

![image](PLACEHOLDER FOR SENSOR DASHBOARD VIEW)

---

## 🧲 9. Smart Switches Behind Legacy Switches [↑](#-table-of-contents)

- Zigbee relays that work behind wall switches  
- Advantages when you can’t replace switch panel  
- Zero line requirement  
- Wiring logic diagram + real photo

![image](PLACEHOLDER FOR INSTALL PHOTO + SIMPLE WIRING SKETCH)

---

## 🗺️ 10. Mapping Light Entities on Floorplans [↑](#-table-of-contents)

- Floorplancreator → image export  
- Using `picture-elements` card  
- Adding light icons + state badges  
- Click-to-toggle logic

![image](PLACEHOLDER FOR FLOORPLAN UI EXAMPLE)

---

## 🧩 11. Final Automation Architecture & Examples [↑](#-table-of-contents)

- Combining motion, presence, lux, timers, and manual override  
- Rooms with multiple sensors  
- Example YAML automation  
- Example Node-RED flow (optional)

```yaml
alias: Bathroom Light - Motion & LUX
trigger:
  - platform: state
    entity_id: binary_sensor.bathroom_motion
    to: 'on'
condition:
  - condition: numeric_state
    entity_id: sensor.bathroom_lux
    below: 15
action:
  - service: light.turn_on
    target:
      entity_id: light.bathroom
```

---

## 🎬 12. Live Video Demonstration [↑](#-table-of-contents)

_A full video demo walking through the house:_

- Corridor lights auto on/off with presence
- Lights staying on when user interacts manually
- Delayed shutoff if no movement
- Floorplan and dashboard views in action

![image](PLACEHOLDER FOR YOUTUBE THUMBNAIL)

---

## 🧠 13. Lessons Learned & Next-Level Ideas [↑](#-table-of-contents)

- Don’t trust cheap Wi-Fi  
- Context-awareness is key  
- Presence > motion  
- Future ideas:
  - iBeacon tracking
  - User-specific routines
  - Adaptive lighting by time of day & person

---

## 🪪 14. License [↑](#-table-of-contents)

MIT or CC BY-NC-SA — pick your preferred open license.

---

## 👨‍💻 15. Author and Related Projects [↑](#-table-of-contents)

_Alexei aka Technik_  
See also:  
- [AJAX Sensor Integration for Automations](https://github.com/AlexeiakaTechnik/Use-Ajax-Security-alarm-sensors-as-a-Automation-Triggers-in-Home-Assistant)  
- [HA Dashboards for Tablets/Phones](https://github.com/AlexeiakaTechnik/Practial-and-stylish-Home-Assistant-Dashboards-for-Tablets-and-Mobile-Phones)
