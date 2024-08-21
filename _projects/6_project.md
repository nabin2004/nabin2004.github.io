---
layout: page
title: Wild-Watch
description: First DataCrunch Competition Winning Project :-)
img:
importance: 4
category: fun
---

# Wild Watch: Early Animal Detection for Ultimate Protection

![demo](https://github.com/abhash-rai/wild-watch/assets/106548397/c6b3ae69-bbe5-409f-ad2d-2e3170d809e0)

Wild watch aims to boost safety along Nepal's wildlife habitats bordering human settlements using object detection technique. It spots dangerous animals and alerts authorities or nearby communities for swift action, focusing on areas near national parks, wildlife reserves, and forests.

It uses a custom-trained YOLOv8 model. This model is deployed on a Flask server for processing of live feed, which is then recieved by a react app and an alert system. Alert system seamlessly integrates IoT components like LCD displays and buzzers, enabling timely alerts for proactive measures.

<br>

# Content

- [Prerequisites](#prerequisites)
- [Yolo model](#yolo-model)
- [Server](#server)
- [Frontend](#frontend)
- [Alert System](#alert-system)
- [Contributors](#contributors)

<br>

# Prerequisites

- You must have `Python 3` installed on your system.
- You must have `Git` installed on your system.
- **(Optional)** You should have `Node.js` (npm) installed on your system (if using development server)

<br>

# Yolo model

- Data was collected, augmented and annotated. You can find it here: <a href='https://drive.google.com/drive/folders/1m8P8-fzJ1uP6EHWUPU4veKx_WV0CSM4I?usp=sharing'>Google Drive</a>
- Yolov8 model architecture was trained on the given data. You can find the model here: ![best.pt](flask-server/best.pt)

<br>

# Server

To run the server, execute the following command:

```
python server.py
```

Certain parameters in the `config.json` file can be changed to customize the system's behavior:
```json
{
    "yolo_model_path": "best.pt",
    "url": 0,
    "scale_factor": 0.5,
    "confidence_threshold": 0.65,
    "esp32_api": "http://192.168.23.188/api/animal-detected"
}
```

- `yolo_model_path`: This parameter denotes the file path to a YOLOv8 model; crucial for identifying animals during video streams.

- `url`: Denotes the URL of video feed or physical camera index related to camera input. For example: "0" suggests it represents the default camera source.

- `scale_factor`: Determines the scaling factor applied to input images before inferencing by the model. For example: value of 0.5 means 50% of the orignal input width and height will be used to feed to the model.

- `confidence_threshold`: Sets the minimum confidence level required for object detection predictions to be considered valid. A threshold means predictions must have a confidence score of threshold or higher to be accepted.

- `esp32_api`: Specifies the endpoint of an API hosted on an ESP32 device. This API is further used for an alarm system, facilitating communication between the detection system and downstream processes for further proactive measures.

## Ports and Endpoints

Ensure that the backend server is running on port 5000 for the component to fetch the video feed and output data. The following ports and endpoints can be used to fetch data:

- **Video Feed**: `http://localhost:5000/video_feed`
- **Output list of detected animals**: `http://localhost:5000/output_data`

<br>

# Frontend

The video feed and output data from the backend server are displayed in a react app. It communicates with the server to fetch the video feed and real-time output data using HTTP requests. To run the frontend, follow these steps:

1. Ensure that the server is running. If not, follow the instructions in the previous section to start the server.

2. Navigate to the frontend directory in your terminal:
   ```
   cd wild-watch/react-app/development-server/wildwatch-app
   ```
3. Install dependencies by running:
   ```
   npm install
   ```
4. Once the dependencies are installed, start the frontend application with:
   ```
   npm start
   ```
5. Open your web browser and enter `http://localhost:3000`.

<br>

# Alert System

![alar-system](https://github.com/abhash-rai/wild-watch/assets/106548397/0e3b5548-ef95-4de2-baaf-73d05883b6eb)

The alarm system, implemented on the ESP32 platform, is designed to enhance wildlife monitoring efforts by providing real-time alerts when animals are detected. The system utilizes two main hardware components: an LCD display for visual alerts and a buzzer for audible alerts. Below documents details about the setup and functionality of the IoT components, including the LCD display and buzzer, specifically for the ESP32 platform.

## Hardware Components

1. `ESP32 Microcontroller`: The ESP32 microcontroller serves as the central processing unit for the IoT alarm system. Itreceives input from the server and manages the communication with other hardware components, ensuring alerting when required.
   
3. `LCD Display (16x2)`: The 16x2 I2C LCD display is used to provide visual alerts to users. It displays messages such as "No Animal Detected," "Single Animal Detected," or "Multiple Animals Detected" based on the detection results.
   
5. `Buzzer`: The buzzer is employed to provide audible alerts when animals are detected. It emits a sound to alert users of potential wildlife activity, ensuring that alerts are not missed even in noisy environments.
   
## Hardware Setup

1. **ESP32 Setup**:

   - Connect the ESP32 microcontroller to the necessary power source and ensure it is programmed with the appropriate firmware to control the LCD display and buzzer.

2. **LCD Display Wiring**:

   - Connect the 16x2 LCD display to the ESP32 microcontroller using the following wiring:
     - VCC to 5V
     - GND to GND
     - SDA to GPIO pin (e.g., GPIO21)
     - SCL to GPIO pin (e.g., GPIO22)

3. **Buzzer Wiring**:
   - Connect the buzzer to the ESP32 microcontroller using the following wiring:
     - Positive (+) terminal to GPIO pin (e.g., GPIO13)
     - Negative (-) terminal to GND

## Functionality

1. **ESP32 Logic**:

   - The ESP32 microcontroller receives input from sensors or image processing modules to detect animals.
   - It processes the detection results and triggers the appropriate actions on the LCD display and buzzer based on the detection outcome.

2. **LCD Display Operation**:

   - Upon detecting animals, the ESP32 sends signals to the LCD display to update the displayed message.
   - If no animals are detected, the display shows "No Animal Detected."
   - If a single animal is detected, the display shows the name of the animal followed by "Detected."
   - If multiple animals are detected, the display shows "Multiple Animals Detected."

3. **Buzzer Activation**:
   - When animals are detected, the ESP32 activates the buzzer to emit a sound alert.
   - The buzzer continues to sound for a predefined duration to ensure that users are alerted to the presence of animals.

## Code Snippets

- [Esp32_server.c++](IoT/Esp32_server.c++): contains the script used in esp32 of the alarm system

- [test_iot_server.py](IoT/test_iot_server.py): contains the testing script used to send signal to the alaram system to activate alarming


<br>

# Contributors

| Name              | Contribution          | Linkedin Profile                              |
|:-----------------:|:---------------------:|:---------------------------------------------:|
| **Abhash Rai**    | Team Lead & Backend | [Abhash Rai](https://www.linkedin.com/in/abhash-rai/) |
| **Nabin Oli**     | Machine Learning      | [Nabin Oli](https://www.linkedin.com/in/nabinoli/) |
| **Bishesh Giri**  | Frontend              | [Bishesh Giri](https://www.linkedin.com/in/bisheshgiri/) |
| **Sudeep Fullel** | IOT                   | [Sudeep Fullel](https://www.linkedin.com/in/sudeepfullel/) |
| **Sanket Shrestha** | Documentation       | [Sanket Shrestha](https://www.linkedin.com/in/sanketstha/) |
| **Shankar Tamang**  | Documentation       | [Shankar Tamang](https://www.linkedin.com/in/shankartamang/) |
