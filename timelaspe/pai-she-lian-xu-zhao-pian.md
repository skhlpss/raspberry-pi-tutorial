# 拍攝連續照片

&#x20;開新檔案 `continuous_images.py` ，連接 2 個按扭到 `GPIO5` 及 `GPIO6` 。改變 `ln9` 及 `ln10` 中 `wait` 及 `no_of_images` 的數值，

{% code title="continuous_images.py" lineNumbers="true" %}
```python
from gpiozero import Button
from signal import pause
from time import sleep 
from io import BytesIO

preview_button = Button(5)
capture_button = Button(6)
captureDir = '/home/pi/timelapse'
wait = 2 
no_of_images = 30

def continuous_capture():
   stream = BytesIO()
   frames = camera.capture_continuous(
       stream,
       format='jpeg',
       use_video_port=True 
   )

   for (number, _) in enumerate(frames, 1):
       Image.open(stream).save(f'{captureDir}/{number:03d}.jpg')
       stream.seek(0)
       stream.truncate()
       print(number)
       sleep(wait)

       if number == no_of_images:
           break
           
preview_button.when_pressed = camera.start_preview
capture_button.when_pressed = continuous_capture
print("The Program is running")
pause()
```
{% endcode %}



