# RPi Streaming Camera

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y python3-flask python3-opencv python3-picamera2
sudo mkdir /scripts
sudo touch launch.sh camera.py
sudo chmod +x /scripts/launch.sh
sudo nano /scripts/camera.py
```

Paste the below code into [camera.py](http://camera.py) and save (CTRL+X and return).

```python
from flask import Flask, Response
from picamera2 import Picamera2
import cv2

app = Flask(__name__)

camera = Picamera2()
camera.configure(camera.create_preview_configuration(main={"format": 'XRGB8888', "size": (640, 480)}))
camera.start()

def generate_frames():
    while True:
        frame = camera.capture_array()
        ret, buffer = cv2.imencode('.jpg', frame)
        frame = buffer.tobytes()
        yield (b'--frame\r\n'
               b'Content-Type: image/jpeg\r\n\r\n' + frame + b'\r\n')

@app.route('/video_feed')
def video_feed():
    return Response(generate_frames(), mimetype='multipart/x-mixed-replace; boundary=frame')

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

Add the below code to launcher.sh

```bash
#!/bin/bash
cd /scripts
sudo python3 camera.py
cd /
```

Add a entry to crontab to run script at startup.

```bash
sudo crontab -e
@reboot bash /scripts/launch.sh >/var/log/camera.log 2>&1
```

Press CTRL+X and return to save

To find the IP address execute the following:

```bash
sudo ip a
```

Reboot to test.

```bash
sudo reboot
```

Navigate to [http://<IP ADDRESS>:5000/video_feed](http://192.168.1.191:5000/video_feed)

## References

[Beginner Tutorial: How to Stream Video from Raspberry Pi Camera to Local Computer using Python (P3)](https://www.youtube.com/watch?v=NOAY1aaVPAw&list=WL&index=36)

[Raspberry Pi: Launch Python Script on Startup](https://www.instructables.com/Raspberry-Pi-Launch-Python-script-on-startup/)
