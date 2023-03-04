---
description: https://gpiozero.readthedocs.io/en/stable/recipes.html#distance-sensor
---

# Ex5 - Distance Sensor

## 任務 1

另開新檔 `distance.py` 利用以程式碼使 `Distance Sensor` 檢測到最近物體的距離

<img src="https://em-content.zobj.net/thumbs/120/softbank/145/warning-sign_26a0.png" alt="Warning on SoftBank 2014" data-size="line">**同學**<mark style="color:red;">**必須注意**</mark>**四支針腳的名稱，分別是 `VCC`, `Trig`, `Echo`, `GND`，不可以連接錯誤**<img src="https://em-content.zobj.net/thumbs/120/softbank/145/warning-sign_26a0.png" alt="Warning on SoftBank 2014" data-size="line">****

<figure><img src="../.gitbook/assets/distance.png" alt=""><figcaption></figcaption></figure>

{% code title="distance.py" lineNumbers="true" %}
```python
from gpiozero import DistanceSensor
from time import sleep

trig = 5
echo = 6

sensor = DistanceSensor(echo, trig)

while True:
    print(sensor.distance * 100, 'cm')
    sleep(1)
```
{% endcode %}

## 任務 2

另開新檔 `distance_2.py`，以下程式碼為當有東西靠近傳感器時，LED 會亮。按以下程式碼連接所需裝置。

{% code title="distance_2.py" lineNumbers="true" %}
```python
from gpiozero import DistanceSensor, LED
from signal import pause

trig = 5
echo = 6
led = LED(26)

sensor = DistanceSensor(
    echo, 
    trig,
    max_distance=1,
    threshold_distance=0.2
)

sensor.when_in_range = led.on
sensor.when_out_of_range = led.off

pause()
```
{% endcode %}

* `threshold` - 啟動值
