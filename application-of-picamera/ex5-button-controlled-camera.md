---
description: >-
  https://gpiozero.readthedocs.io/en/stable/recipes.html#button-controlled-camera
---

# Ex1 - Button controlled camera

## 任務 1

<figure><img src="../.gitbook/assets/button.png" alt=""><figcaption></figcaption></figure>

另開新檔 `button_camera.py`。

以下程式碼，讓你在按下按扭時，使PiCamera 拍下照片，並把照片儲存到 `/home/pi` 中。

{% code title="button_camera.py" lineNumbers="true" %}
```python
from gpiozero import Button
from picamera import PiCamera
from datetime import datetime
from signal import pause

button = Button(5)
camera = PiCamera()

def capture():
    timestamp = datetime.now().isoformat()
    camera.capture(f"/home/pi/{timestamp}.jpg")

button.when_pressed = capture

pause()
```
{% endcode %}

## 任務 2

另開新檔 `button_camera_2.py`。試按以下程式碼連接所需裝置，然後細閱程式碼，你預計會出現甚麼結果？

{% code title="button_camera_2.py" lineNumbers="true" %}
```python
from gpiozero import Button
from picamera import PiCamera
from datetime import datetime
from signal import pause

left_button = Button(5)
right_button = Button(6)
camera = PiCamera()

def capture():
    timestamp = datetime.now().isoformat()
    camera.capture('/home/pi/%s.jpg' % timestamp)
    camera.stop_preview()

left_button.when_pressed = camera.start_preview
right_button.when_pressed = capture

pause()
```
{% endcode %}
