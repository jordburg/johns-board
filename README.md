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

# Create a new virtual environment (if not already done)
python3 -m venv ~/led_control_env

# Activate the virtual environment
source ~/led_control_env/bin/activate

# Install the required Python packages
pip install rpi_ws281x adafruit-circuitpython-neopixel flask adafruit-blinka

# Edit the service file
sudo nano /etc/systemd/system/led_controller.service

# Add the following content to the service file
# (Use the content provided above)

# Reload systemd to apply the new service
sudo systemctl daemon-reload
sudo systemctl enable led_controller.service
sudo systemctl start led_controller.service
sudo systemctl status led_controller.service


# Install the required Python packages
pip install rpi_ws281x adafruit-circuitpython-neopixel flask adafruit-blinka

sudo -E ~/led_control_env/bin/python ~/led_control_project/led_controller.py

[Unit]
Description=LED Controller Service
After=multi-user.target

[Service]
ExecStart=sudo -E /home/jkenagy/led_control_env/bin/python /home/jkenagy/led_control_project/led_controller.py
WorkingDirectory=/home/jkenagy/led_control_project
StandardOutput=inherit
StandardError=inherit
Restart=always
User=jkenagy

[Install]
WantedBy=multi-user.target

sudo systemctl daemon-reload
sudo systemctl enable led_controller.service
sudo systemctl start led_controller.service
sudo systemctl status led_controller.service

To prevent the LED controller service from starting up on boot, you need to disable and optionally remove the systemd service file you created. Here are the steps:

### 1. Disable the Service

Disabling the service will prevent it from starting automatically on boot.

```bash
sudo systemctl disable led_controller.service
```

### 2. Stop the Service (if it is running)

If the service is currently running, stop it.

```bash
sudo systemctl stop led_controller.service
```

### 3. Remove the Service File (optional)

If you want to completely remove the service file from your system, you can delete it.

```bash
sudo rm /etc/systemd/system/led_controller.service
```

### 4. Reload systemd

After deleting the service file, reload the systemd manager configuration to apply the changes.

```bash
sudo systemctl daemon-reload
```

### Summary of Commands

Here is the complete sequence of commands:

```bash
# Disable the service
sudo systemctl disable led_controller.service

# Stop the service if it is running
sudo systemctl stop led_controller.service

# Remove the service file (optional)
sudo rm /etc/systemd/system/led_controller.service

# Reload systemd to apply the changes
sudo systemctl daemon-reload
```

By following these steps, you will ensure that the LED controller service does not start automatically on boot and, if desired, completely remove the service file from your system.

import board
import neopixel
import sys

# Configuration
LED_PIN = board.D18
NUM_LEDS = 2000  # Total number of LEDs
ORDER = neopixel.GRB

# Create NeoPixel object
pixels = neopixel.NeoPixel(LED_PIN, NUM_LEDS, auto_write=False, pixel_order=ORDER)

# Set brightness
brightness = float(sys.argv[1])  # Get brightness value from command line argument
if 0.0 <= brightness <= 1.0:
    pixels.brightness = brightness
    pixels.show()
    print(f"Brightness set to {brightness}")
else:
    print("Brightness value must be between 0.0 and 1.0")

Traceback (most recent call last):
  File "/home/jkenagy/led_brightness.py/led_brightness.py", line 14, in <module>
    brightness = float(sys.argv[1])  # Get brightness value from command line argument
                       ~~~~~~~~^^^
IndexError: list index out of range

ss.py 1.0
Can't open /dev/mem: Permission denied
Traceback (most recent call last):
  File "/home/jkenagy/led_brightness.py/led_brightness.py", line 17, in <module>
    pixels.show()
  File "/home/jkenagy/led_control_env/lib/python3.11/site-packages/adafruit_pixelbuf.py", line 204, in show
    return self._transmit(self._post_brightness_buffer)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/jkenagy/led_control_env/lib/python3.11/site-packages/neopixel.py", line 180, in _transmit
    neopixel_write(self.pin, buffer)
  File "/home/jkenagy/led_control_env/lib/python3.11/site-packages/neopixel_write.py", line 46, in neopixel_write
    return _neopixel.neopixel_write(gpio, buf)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/jkenagy/led_control_env/lib/python3.11/site-packages/adafruit_blinka/microcontroller/bcm283x/neopixel.py", line 78, in neopixel_write
    raise RuntimeError(
RuntimeError: NeoPixel support requires running with sudo, please try again!
swig/python detected a memory leak of type 'ws2811_t *', no destructor found.

The error message indicates that you need to run the script with `sudo` to access the necessary hardware resources on the Raspberry Pi.

### Adjusting the Script Execution

To run the script with `sudo` while ensuring it uses the virtual environment, you need to specify the full path to the Python interpreter within your virtual environment.

### Steps to Run the Script with `sudo`

1. **Ensure the Script is in the Correct Location**

   Make sure your script is located where you expect it to be. For example, let's assume your script is at `/home/jkenagy/led_brightness.py`.

2. **Activate the Virtual Environment**

   Activate the virtual environment to find the correct Python interpreter path.

   ```bash
   source ~/led_control_env/bin/activate
   ```

3. **Find the Full Path of the Python Interpreter**

   Use the `which` command to get the full path of the Python interpreter in your virtual environment.

   ```bash
   which python
   ```

   This should return something like `/home/jkenagy/led_control_env/bin/python`.

4. **Run the Script with `sudo`**

   Use `sudo` to run the script, ensuring you use the full path to the Python interpreter from your virtual environment.

   ```bash
   sudo /home/jkenagy/led_control_env/bin/python /home/jkenagy/led_brightness.py 1.0
   ```

### Example Script (`led_brightness.py`)

Here is an example of the `led_brightness.py` script for reference:

```python
import board
import neopixel
import sys

# Configuration
LED_PIN = board.D18
NUM_LEDS = 2000  # Total number of LEDs
ORDER = neopixel.GRB

# Create NeoPixel object
pixels = neopixel.NeoPixel(LED_PIN, NUM_LEDS, auto_write=False, pixel_order=ORDER)

# Set brightness
brightness = float(sys.argv[1])  # Get brightness value from command line argument
if 0.0 <= brightness <= 1.0:
    pixels.brightness = brightness
    pixels.show()
    print(f"Brightness set to {brightness}")
else:
    print("Brightness value must be between 0.0 and 1.0")
```

### Running the Script

Now, execute the script with `sudo` and the full path to the Python interpreter:

```bash
sudo /home/jkenagy/led_control_env/bin/python /home/jkenagy/led_brightness.py 1.0
```




YYYYYY

If you want to control the brightness of the LEDs directly from the terminal without using a specific board library like `board` from Adafruit, you can still use the `rpi_ws281x` library, which provides direct control over the GPIO pins. Below is a step-by-step guide to achieve this.

### Step-by-Step Guide

1. **Install the `rpi_ws281x` Library**

   First, make sure you have the `rpi_ws281x` library installed in your environment. You can install it using `pip`:

   ```bash
   pip install rpi-ws281x
   ```

2. **Create a Python Script for Setting Brightness**

   Create a new Python script, for example, `set_brightness.py`:

   ```bash
   nano set_brightness.py
   ```

3. **Add the Following Code to `set_brightness.py`**

   This script will set the brightness of the LEDs connected to a specified GPIO pin:

   ```python
   import sys
   from rpi_ws281x import PixelStrip, Color

   # Configuration
   LED_COUNT = 2000       # Number of LED pixels.
   LED_PIN = 18           # GPIO pin connected to the pixels (18 uses PWM).
   LED_FREQ_HZ = 800000   # LED signal frequency in hertz (usually 800khz)
   LED_DMA = 10           # DMA channel to use for generating signal (try 10)
   LED_BRIGHTNESS = 255   # Set to 0 for darkest and 255 for brightest
   LED_INVERT = False     # True to invert the signal (when using NPN transistor level shift)
   LED_CHANNEL = 0        # Set to '1' for GPIOs 13, 19, 41, 45 or 53

   # Create PixelStrip object with appropriate configuration.
   strip = PixelStrip(LED_COUNT, LED_PIN, LED_FREQ_HZ, LED_DMA, LED_INVERT, LED_BRIGHTNESS, LED_CHANNEL)
   # Intialize the library (must be called once before other functions).
   strip.begin()

   # Set brightness from command line argument
   brightness = int(float(sys.argv[1]) * 255)
   if 0 <= brightness <= 255:
       strip.setBrightness(brightness)
       strip.show()
       print(f"Brightness set to {brightness / 255.0}")
   else:
       print("Brightness value must be between 0.0 and 1.0")
   ```

4. **Save and Exit**:

   Save the file by pressing `Ctrl + X`, then `Y`, and `Enter`.

5. **Run the Script with `sudo`**

   Since the script interacts with the GPIO pins directly, you need to run it with `sudo`:

   ```bash
   sudo python3 set_brightness.py 0.5
   ```

   Replace `0.5` with the desired brightness value between `0.0` (off) and `1.0` (full brightness).

### Example Commands

- **Create the Script**:

  ```bash
  nano set_brightness.py
  ```

- **Add the Code**:

  ```python
  import sys
  from rpi_ws281x import PixelStrip, Color

  # Configuration
  LED_COUNT = 2000       # Number of LED pixels.
  LED_PIN = 18           # GPIO pin connected to the pixels (18 uses PWM).
  LED_FREQ_HZ = 800000   # LED signal frequency in hertz (usually 800khz)
  LED_DMA = 10           # DMA channel to use for generating signal (try 10)
  LED_BRIGHTNESS = 255   # Set to 0 for darkest and 255 for brightest
  LED_INVERT = False     # True to invert the signal (when using NPN transistor level shift)
  LED_CHANNEL = 0        # Set to '1' for GPIOs 13, 19, 41, 45 or 53

  # Create PixelStrip object with appropriate configuration.
  strip = PixelStrip(LED_COUNT, LED_PIN, LED_FREQ_HZ, LED_DMA, LED_INVERT, LED_BRIGHTNESS, LED_CHANNEL)
  # Intialize the library (must be called once before other functions).
  strip.begin()

  # Set brightness from command line argument
  brightness = int(float(sys.argv[1]) * 255)
  if 0 <= brightness <= 255:
      strip.setBrightness(brightness)
      strip.show()
      print(f"Brightness set to {brightness / 255.0}")
  else:
      print("Brightness value must be between 0.0 and 1.0")
  ```

- **Run the Script**:

  ```bash
  sudo python3 set_brightness.py 0.5
  ```

By using this script, you can directly control the brightness of your LEDs from the terminal. This approach uses the `rpi_ws281x` library, which is specifically designed for controlling WS281x LEDs on a Raspberry Pi.

This command should adjust the brightness of your LEDs to the value specified (1.0 in this case).

By following these steps, you ensure that the script has the necessary permissions to access the hardware resources while using the correct Python interpreter from your virtual environment.

import sys
from rpi_ws281x import PixelStrip, Color

# Configuration
LED_COUNT = 2000       # Number of LED pixels.
LED_PIN = 18           # GPIO pin connected to the pixels (18 uses PWM).
LED_FREQ_HZ = 800000   # LED signal frequency in hertz (usually 800khz)
LED_DMA = 10           # DMA channel to use for generating signal (try 10)
LED_BRIGHTNESS = 255   # Set to 0 for darkest and 255 for brightest
LED_INVERT = False     # True to invert the signal (when using NPN transistor level shift)
LED_CHANNEL = 0        # Set to '1' for GPIOs 13, 19, 41, 45 or 53

# Create PixelStrip object with appropriate configuration.
strip = PixelStrip(LED_COUNT, LED_PIN, LED_FREQ_HZ, LED_DMA, LED_INVERT, LED_BRIGHTNESS, LED_CHANNEL)
# Intialize the library (must be called once before other functions).
strip.begin()

# Define colors
COLORS = {
    "red": Color(255, 0, 0),
    "green": Color(0, 255, 0),
    "blue": Color(0, 0, 255),
    "purple": Color(128, 0, 128)
}

# Function to set color
def set_color(color_name):
    color = COLORS.get(color_name.lower(), Color(0, 0, 0))
    for i in range(strip.numPixels()):
        strip.setPixelColor(i, color)
    strip.show()

# Main logic
if len(sys.argv) == 2:
    # Set brightness
    brightness = float(sys.argv[1])
    if 0.0 <= brightness <= 1.0:
        strip.setBrightness(int(brightness * 255))
        strip.show()
        print(f"Brightness set to {brightness}")
    else:
        print("Brightness value must be between 0.0 and 1.0")
elif len(sys.argv) == 3 and sys.argv[1] == "color":
    # Set color
    color_name = sys.argv[2]
    set_color(color_name)
    print(f"Color set to {color_name}")
else:
    print("Usage: sudo python3 set_brightness_and_color.py <brightness> | color <color_name>")


