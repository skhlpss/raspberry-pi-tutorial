# 拍攝連續照片

開新檔案 `continuous_images.py` ，連接2個按扭到 `GPIO5` 及 `GPIO6`， 分別用作控制預覽 `preview` 及啟動連續拍照 `continuous_capture`。

{% code title="continuous_images.py" lineNumbers="true" %}
```python
from gpiozero import Button
from picamera import PiCamera
from signal import pause
from PIL import Image
from time import sleep
from datetime import datetime
from io import BytesIO
from os import makedirs, path

preview_button = Button(5)
capture_button = Button(6)
camera = PiCamera()

captureDir = '/home/pi/Desktop/timelapse'

if not path.exists(captureDir):
    makedirs(captureDir)

wait = float(input('每張相片相隔多少秒？ '))
no_of_images = int(input('總共要拍多少張相片？ '))

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
       print(filename, datetime.now())
       sleep(wait)

       if number == no_of_images:
           break
           
preview_button.when_pressed = camera.start_preview
preview_button.when_released = camera.stop_preview
capture_button.when_pressed = continuous_capture
```
{% endcode %}



