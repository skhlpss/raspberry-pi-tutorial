# 將相片轉換成影片

<figure><img src="../.gitbook/assets/terminal.png" alt=""><figcaption></figcaption></figure>

打開 `Terminal` 並執行以下程式

{% code lineNumbers="true" %}
```sh
# 安將所須軟件

sudo apt update
sudo apt install imagemagick
```
{% endcode %}

```sh
cd /home/pi/timelapse

# Merge all images to videos
# -r 每一秒的 frame rate 
ffmpeg -r 18 -i ./%03d.jpg output.mp4
```
