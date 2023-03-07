# 將相片轉換成影片

<figure><img src="../.gitbook/assets/terminal.png" alt=""><figcaption></figcaption></figure>

打開 `Terminal` ，執行以下指令，以安裝所需程式 `imagemagick`

{% code lineNumbers="true" %}
```sh
# 安將所須軟件
sudo apt update
sudo apt install imagemagick
```
{% endcode %}

打開 `Terminal` 並輸入以下指令，以下例子是製作一段 18 FPS 的影片。

```sh
# 到以下文件夾，cd 代表 change directory
cd /home/pi/Desktop/timelapse

# 顯示所有文件，ls 代表 list
ls

# 將所有相片轉成影片，並儲存在 ../ ，即是 timelapse 的上一層 Desktop
# 檔案名為 output.mp4
# -r 代表每一秒的 frame rate 
# -i 代表滙入檔案的格式，%3d 表示3位數字
ffmpeg -r 18 -i ./%03d.jpg ../output.mp4
```
