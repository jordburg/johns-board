# LED Control Project

## Overview
This project allows you to control addressable LEDs using a Raspberry Pi via a web interface. Users can set the color of each LED, save configurations, and load saved configurations.

## Setup
1. Install the required libraries:
    ```bash
    sudo apt-get install python3-pip
    sudo pip3 install -r requirements.txt
    ```
2. Run the Python script:
    ```bash
    python3 led_controller.py
    ```
3. Access the web interface at `http://<Raspberry_Pi_IP_Address>:5000`.

## Directory Structure
```plaintext
led_control_project/
├── Database/
│   ├── index.html
│   └── some_configuration.json
├── led_controller.py
└── README.md

# Create a new virtual environment (if not already done)
python3 -m venv ~/led_control_env

# Activate the virtual environment
source ~/led_control_env/bin/activate

# Install the required Python packages
pip install rpi_ws281x adafruit-circuitpython-neopixel flask adafruit-blinka

sudo -E ~/led_control_env/bin/python ~/led_control_project/led_controller.py

