# Using_PYnput
PYnput module  in python helps you to control your computer hardware just with the code.
pynput is a Python library that makes it incredibly easy to control and monitor input devices such as your mouse and keyboard. With pynput, you can press keys on your keyboard, or move your mouse, all from your Python program!
# pynput Library Documentation

`pynput` allows you to control and monitor input devices. Currently, mouse and keyboard input and monitoring are supported.

## Controlling the Mouse

### Importing the Controller
```python
# Import Button and Controller from pynput.mouse
from pynput.mouse import Button, Controller

# Create a mouse controller
mouse = Controller()
```
Reading Pointer Position
```

# Read the current pointer position
print('The current pointer position is {0}'.format(mouse.position))
Setting Pointer Position

# Set the pointer to a specific position
mouse.position = (10, 20)
print('Now we have moved it to {0}'.format(mouse.position))
Moving the Pointer

# Move the pointer relative to its current position
mouse.move(5, -5)
Clicking the Mouse

# Press and release the left mouse button
mouse.press(Button.left)
mouse.release(Button.left)

# Double click the left mouse button
mouse.click(Button.left, 2)

# Scroll the mouse
mouse.scroll(0, 2)  # Scroll two steps down
Monitoring the Mouse

# Import the mouse listener
from pynput import mouse

# Define callback functions for mouse events
def on_move(x, y):
    print('Pointer moved to {0}'.format((x, y)))

def on_click(x, y, button, pressed):
    print('{0} at {1}'.format('Pressed' if pressed else 'Released', (x, y)))
    if not pressed:
        # Stop listener
        return False

def on_scroll(x, y, dx, dy):
    print('Scrolled {0} at {1}'.format('down' if dy < 0 else 'up', (x, y)))
Starting the Listener
python
Copy code
# Collect events until released
with mouse.Listener(
        on_move=on_move,
        on_click=on_click,
        on_scroll=on_scroll) as listener:
    listener.join()

```
```
# Start listener in a non-blocking fashion
listener = mouse.Listener(
    on_move=on_move,
    on_click=on_click,
    on_scroll=on_scroll)
listener.start()
```
Handling Errors in Mouse Listener
```
# Example of handling errors within a mouse listener
from pynput import mouse

class MyException(Exception): 
    pass

def on_click(x, y, button, pressed):
    if button == mouse.Button.left:
        raise MyException(button)

# Collect events until released with error handling
with mouse.Listener(on_click=on_click) as listener:
    try:
        listener.join()
    except MyException as e:
        print('{0} was clicked'.format(e.args[0]))
Controlling the Keyboard
```
Importing the Keyboard Controller
```
# Import Key and Controller from pynput.keyboard
from pynput.keyboard import Key, Controller

# Create a keyboard controller
keyboard = Controller()
Typing and Pressing Keys
python
Copy code
# Press and release space
keyboard.press(Key.space)
keyboard.release(Key.space)

# Type a lower case 'a'
keyboard.press('a')
keyboard.release('a')

# Type an upper case 'A'
keyboard.press('A')
keyboard.release('A')

# Type 'Hello World' using the shortcut type method
keyboard.type('Hello World')
Monitoring the Keyboard
```
Setting Up a Keyboard Listener
```
from pynput import keyboard

# Define callback functions for keyboard events
def on_press(key):
    try:
        print('alphanumeric key {0} pressed'.format(key.char))
    except AttributeError:
        print('special key {0} pressed'.format(key))

def on_release(key):
    print('{0} released'.format(key))
    if key == keyboard.Key.esc:
        # Stop listener
        return False
```
Starting the Keyboard Listener
```
# Collect events until released
with keyboard.Listener(
        on_press=on_press,
        on_release=on_release) as listener:
    listener.join()
```
Non-Blocking Keyboard Listener

```
# Start listener in a non-blocking fashion
listener = keyboard.Listener(
    on_press=on_press,
    on_release=on_release)
listener.start()
```
Handling Errors in Keyboard Listener
```
# Example of handling errors within a keyboard listener
from pynput import keyboard

class MyException(Exception): 
    pass

def on_press(key):
    if key == keyboard.Key.esc:
        raise MyException(key)

# Collect events until released with error handling
with keyboard.Listener(on_press=on_press) as listener:
    try:
        listener.join()
    except MyException as e:
        print('{0} was pressed'.format(e.args[0]))
```
Global Hotkeys
```
# Using GlobalHotKeys for hotkey combinations
from pynput import keyboard

def on_activate_h():
    print('<ctrl>+<alt>+h pressed')

def on_activate_i():
    print('<ctrl>+<alt>+i pressed')

with keyboard.GlobalHotKeys({
        '<ctrl>+<alt>+h': on_activate_h,
        '<ctrl>+<alt>+i': on_activate_i}) as h:
    h.join()
```
