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
# Initialize the library (must be called once before other functions).
strip.begin()

# Function to clear the LEDs
def clear_leds():
    for i in range(strip.numPixels()):
        strip.setPixelColor(i, Color(0, 0, 0))
    strip.show()

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

