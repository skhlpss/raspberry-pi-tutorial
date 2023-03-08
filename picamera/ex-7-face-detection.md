# Ex 7 Face Detection

&#x20;打開 `Terminal`

<figure><img src="../.gitbook/assets/terminal.png" alt=""><figcaption></figcaption></figure>

輸入以下指令，以安裝所須套件，安裝時間頗長，請耐心等候。

```sh
sudo apt-get install libatlas-base-dev
pip3 install opencv-python
pip3 install face_recognition
```

開啟新檔 `face_detection.py` ，將 `LED` 連接到 `GPIO5` 然後執行程式。

{% code title="face_detection.py" lineNumbers="true" %}
````python
```python
from picamera import PiCamera
from gpiozero import LED
from time import sleep
import face_recognition
import cv2
import numpy as np

camera = PiCamera()
led = LED(5)

# 用作儲存已登記的 face_encoding 及 name
known_face_encodings = []
known_face_names = []

username = input('使用者名稱 (按空白跳過)： ')

if username == '':
    exit()

while username != '':
    camera.start_preview()
    image_path = f'/home/pi/{username}.jpg'
    sleep(5)
    camera.capture(image_path)
    sleep(1)
    camera.stop_preview()
    # Load a sample picture and learn how to recognize it.
    user_image = face_recognition.load_image_file(image_path)
    face_encoding = face_recognition.face_encodings(user_image)[0]

    known_face_encodings.append(face_encoding)
    known_face_names.append(username)

    username = input('使用者名稱 (按空白跳過)： ')

camera.close()

# Get a reference to webcam #0 (the default one)
video_capture = cv2.VideoCapture(0)

# Initialize some variables
face_locations = []
face_encodings = []
face_names = []
process_this_frame = True
UNKNOWN = "Unknown"

while True:
    # Grab a single frame of video
    ret, frame = video_capture.read()

    # Only process every other frame of video to save time
    if process_this_frame:
        small_frame = cv2.resize(frame, (0, 0), fx=0.25, fy=0.25)

        rgb_small_frame = small_frame[:, :, ::-1]

        face_locations = face_recognition.face_locations(rgb_small_frame)
        face_encodings = face_recognition.face_encodings(
            rgb_small_frame, face_locations
        )

        face_names = []
        for face_encoding in face_encodings:
            # See if the face is a match for the known face(s)
            matches = face_recognition.compare_faces(
                known_face_encodings, face_encoding
            )

            face_distances = face_recognition.face_distance(
                known_face_encodings, face_encoding
            )

            best_match_index = np.argmin(face_distances)

            if matches[best_match_index]:
                name = known_face_names[best_match_index]
            else:
                name = UNKNOWN

            face_names.append(name)

    process_this_frame = not process_this_frame
    
    try:
        face_names.find(UNKNOWN)
        led.on()
    except:
        led.off()

    # Display the results
    for (top, right, bottom, left), name in zip(face_locations, face_names):
        # Scale back up face locations since the frame we detected in was scaled to 1/4 size
        top *= 4
        right *= 4
        bottom *= 4
        left *= 4

        # Draw a box around the face
        cv2.rectangle(frame, (left, top), (right, bottom), (0, 0, 255), 2)

        # Draw a label with a name below the face
        cv2.rectangle(
            frame,
            (left, bottom - 35),
            (right, bottom),
            (0, 0, 255),
            cv2.FILLED
        )
        font = cv2.FONT_HERSHEY_DUPLEX
        cv2.putText(
            frame,
            name,
            (left + 6, bottom - 6),
            font,
            1.0,
            (255, 255, 255),
            1
        )

    # Display the resulting image
    cv2.imshow('Video', frame)

    # Hit 'q' on the keyboard to quit!
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# Release handle to the webcam
video_capture.release()
cv2.destroyAllWindows()

```
````
{% endcode %}
