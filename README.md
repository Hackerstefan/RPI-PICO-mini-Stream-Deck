# RPI-PICO-mini-Stream-Deck
it has 3 buttons, and is scriptable meaning that you can edit what every individual button does.

Parts👻
- raspberry pi pico
- 3+ buttons

Steps🌹
- grab the 3 push buttons
- connect one pin to negative of the pico (GND)
- then connect the other pins to gpio 2, gpio 3 and gpio 4.

Flashing❌
- reset the pico (hold the bootsel button and plug it in)
- download circuit python.uf2 for your pico model
- download circuit python library
- put the .uf2 file on the pico
- wait a bit
- then put the adafruit_hid in the libraries folder.
- put the code into the code.py
- DONE🔥

Code (fedora 43, super key opens ulauncher/search)

import time
import board
import digitalio
import usb_hid
from adafruit_hid.keyboard import Keyboard
from adafruit_hid.keycode import Keycode
from adafruit_hid.keyboard_layout_us import KeyboardLayoutUS
# Setup HID
kbd = Keyboard(usb_hid.devices)
layout = KeyboardLayoutUS(kbd)
# Pins G2, G3, G4 (using internal pull-ups)
pins = [board.GP2, board.GP3, board.GP4]
buttons = []
for pin in pins:
    btn = digitalio.DigitalInOut(pin)
    btn.switch_to_input(pull=digitalio.Pull.UP)
    buttons.append(btn)
def open_app(name):
    """Refined for Fedora + ULauncher"""
    kbd.send(Keycode.GUI)
    time.sleep(0.3)         # Increased wait for ULauncher to gain focus
    layout.write(name)
    time.sleep(0.2)         # Wait for search results to populate
    kbd.send(Keycode.ENTER)
print("Stream Deck Online")
while True:
    # --- GP2: Alt-Tab (Fixed Timing) ---
    if not buttons[0].value:
        kbd.press(Keycode.ALT)
        time.sleep(0.05)
        kbd.send(Keycode.TAB)
        time.sleep(0.1)     # Hold Alt slightly longer for Fedora
        kbd.release_all()
        while not buttons[0].value: time.sleep(0.01)
        # --- GP3: Brave (Fixed Functionality) ---
    if not buttons[1].value:
        open_app("brave")
        while not buttons[1].value: time.sleep(0.01)
        # --- GP4: Equibop ---
    if not buttons[2].value:
        open_app("equibop")
        while not buttons[2].value: time.sleep(0.01)

    time.sleep(0.01)
Useful links:
https://circuitpython.org/
https://circuitpython.org/libraries

Photos🤍
![13513](https://github.com/user-attachments/assets/2afd8508-e2b7-4d3a-997a-2aea238f5235)

![13516](https://github.com/user-attachments/assets/67a55cec-4137-4a00-924b-f4449d94f26c)

