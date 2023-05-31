# 簡介

<figure><img src="../.gitbook/assets/timelapse_example.gif" alt=""><figcaption></figcaption></figure>

利用微型電腦拍攝縮時照片，並透過 Linux Command 程式，將照片合成影片。當中需要計算拍攝照片相隔的時間及相片的數目。

## 甚麼是 FPS

FPS 的意思是 Frame per second，即影片的每一秒中有多少相片，例如一段 24 FPS 的影片，就是每一秒的片段中，均有 24 張圖片。

## 問題

* 如要製作一段縮時影片，影片要將 3 分鐘的拍攝內容壓縮至5秒，影片的 FPS 為 18。請問合共需要多少張相片？應需相隔多少秒才拍一張相片？
  * 你需要的照片數目 $$18 × 5 = 90$$
  * 每張相片須相隔 $$\displaystyle \frac{3\times 60 }{90}= 2$$

## 接駁鏡頭

在進入下一步前，請先按教學[接駁鏡頭](../picamera/connect\_to\_camera.md)。
