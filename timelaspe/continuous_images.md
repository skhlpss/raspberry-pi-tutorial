# 拍攝連續照片

&#x20;開新檔案 `continuous_images.py` ，連接 1 個按扭到 `GPIO5` 及 `GPIO6` 。改變 `ln12` 及 `ln13` 中 `wait` 及 `no_of_images` 的數值，

{% code title="continuous_images.py" lineNumbers="true" %}
```python
from gpiozero import Button
from picamera import PiCamera
from signal import pause
from PIL import Image
from time import sleep
from datetime import datetime
from io import BytesIO

preview_button = Button(5)
capture_button = Button(6)
captureDir = '/home/pi/timelapse'
wait = 2 
no_of_images = 30

def toggle():
    if camera.preview:
        camera.stop_preview()
    else:
        camera.start_preview()

def continuous_capture():
   stream = BytesIO()
   frames = camera.capture_continuous(
       stream,
       format='jpeg',
       use_video_port=True 
   )

   for (number, _) in enumerate(frames, 1):
       filename = f'{captureDir}/{number:03d}.jpg'
       Image.open(stream).save(filename)
       stream.seek(0)
       stream.truncate()
       print(filename, datetime.now)
       sleep(wait)

       if number == no_of_images:
           break
           
preview_button.when_pressed = toggle
capture_button.when_pressed = continuous_capture
print("The Program is running")
```
{% endcode %}



