# 🎯 Real-Time Face Tracking with Stepper Motor & Servo Control

This project implements a **real-time face tracking system** using OpenCV and Raspberry Pi that controls both **stepper motors** and **servo motors** to dynamically adjust orientation based on the detected face position.

---

## 📦 Features

* 🧠 **Face Detection** using Haar cascades
* 🔄 **Object Tracking** with OpenCV's **MOSSE Tracker**
* 🌀 **Dynamic Control**:

  * Stepper motor: angle-based control using GPIO
  * Servo motor: precise tilt/pan via Adafruit’s ServoKit
* 📈 **Live Feedback** of face position and tracking performance (FPS, status)
* 📹 Works with live video feed (USB or Pi Camera)

---

## 🔧 System Architecture

1. **Capture Frame** via camera
2. **Detect Face** using Haar cascade (`haarcascade_frontalface_default.xml`)
3. **Initialize Tracker** (MOSSE) if a face is found
4. **Track Face** and compute:

   * Offset from frame center
   * Angular displacement (theta) in X and Y
5. **Control Hardware**:

   * **Stepper Motor** rotates via GPIO pulses (`motorInput`)
   * **Servo Motor** adjusts via PWM (Adafruit ServoKit)

---

## 🛠️ Requirements

### 📦 Libraries

* `opencv-python`
* `imutils`
* `numpy`
* `adafruit-circuitpython-servokit`
* `RPi.GPIO`

Install with:

```bash
pip install opencv-python imutils numpy adafruit-circuitpython-servokit
```

## 📂 Project Structure

```
📁 Face_Tracking_System/
│
├── face_tracking.py              # Main script
├── haarcascade_frontalface_default.xml  # Face detection XML
└── README.md                     # Project documentation
```

---

## 🎮 Usage

```bash
python face_tracking.py
```

* System begins face detection and tracking automatically.
* Stepper and servo motors move to align with face center.
* Press `q` to quit safely.

---

## ⚙️ Hardware Setup

* **Servo**: Connected via PCA9685 to I2C bus
* **Stepper**:

  * DIR pin → GPIO 10
  * STEP pin → GPIO 8
* Ensure correct power supply to both motors
* Use `BOARD` mode in GPIO (as configured)

---

## 📐 Tuning Parameters

* `thetaX`, `thetaY` are proportional to offset from center.
* Tune with:

  ```python
  thetaX = int(errorPan * constant / width)
  thetaY = int(errorTilt * constant / height)
  ```
* Servo angles: Adjust servo index and neutral angles as per hardware orientation
