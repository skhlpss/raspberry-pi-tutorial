# Ex 7 Face Detection

&#x20;打開 Terminal

<figure><img src="../.gitbook/assets/terminal.png" alt=""><figcaption></figcaption></figure>

下載 face\_detection.xml 至 /home/pi

```sh
curl https://raw.githubusercontent.com/adarsh1021/facedetection/master/haarcascade_frontalface_default.xml -o  /home/pi/face_default.xml
```

安裝 OpenCV

```sh
pip install opencv-python
```

開啟新檔 `face_detection.py` 。複製以下程式碼到 `face_detection.py`，然後執行程式

{% code title="face_detection.py" lineNumbers="true" %}
```python
from cv2 import (
    CascadeClassifier,
    VideoCapture,
    cvtColor,
    rectangle,
    imshow,
    waitKey,
    COLOR_BGR2GRAY
)

# Load the cascade
face_cascade = CascadeClassifier('/home/pi/face_default.xml')

cap = VideoCapture(0)

while True:
    # Read the frame
    _, img = cap.read()
    # Convert to grayscale
    gray = cvtColor(img, COLOR_BGR2GRAY)
    # Detect the faces
    faces = face_cascade.detectMultiScale(gray, 1.1, 4)
    # Draw the rectangle around each face
    for (x, y, w, h) in faces:
        rectangle(img, (x, y), (x+w, y+h), (255, 0, 0), 2)
    # Display
    imshow('img', img)
    # Stop if escape key is pressed
    k = waitKey(30) & 0xff
    if k == 27:
        break

cap.release()

```
{% endcode %}
