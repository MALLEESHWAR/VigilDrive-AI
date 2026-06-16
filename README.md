# VigilDrive AI – AI-Powered Driver Alertness Monitoring System

## Project Description

VigilDrive AI is an AI-powered real-time driver alertness monitoring system that enhances road safety by continuously analyzing driver facial features and behavioral patterns. The system detects signs of driver fatigue and drowsiness using facial landmark analysis, eye closure duration monitoring, and head nodding detection. When signs of drowsiness are identified, an immediate audio alert is generated to warn the driver and help prevent accidents.

The project leverages MediaPipe's pre-trained facial landmark detection model to track facial features in real time. By analyzing Eye Aspect Ratio (EAR) and head movement patterns, VigilDrive AI provides an intelligent and efficient solution for driver safety monitoring.

This project demonstrates practical applications of Artificial Intelligence, Computer Vision, Facial Landmark Detection, and Real-Time Video Processing in the field of intelligent transportation and road safety.

## Features

* **Real-Time Face Detection**: Uses MediaPipe Face Mesh to detect faces and extract 468 facial landmarks
* **Eye Aspect Ratio (EAR) Calculation**: Monitors eye closure by calculating the distance ratio between eye landmarks
* **Head Nod Detection**: Tracks vertical head movements to detect repeated nodding patterns
* **Dual Alertness Monitoring Criteria**:

  * Eyes closed for more than 2 seconds continuously
  * More than 3 head nods within a 10-second window
* **Audio Alarm System**: Generates or plays an alarm sound when driver fatigue is detected
* **Live Feedback Dashboard**: Displays real-time metrics including EAR values, eye closure duration, and nod count
* **Visual Safety Alerts**: Shows "DROWSY! WAKE UP!" warning in red when fatigue is detected

## Technology Stack

* **Python 3.8+**: Programming language
* **OpenCV (cv2)**: Video capture and image processing
* **MediaPipe**: AI-powered facial landmark detection and face mesh tracking
* **NumPy**: Numerical operations and array handling
* **Pygame**: Audio playback for alarm sounds
* **SciPy**: Statistical calculations and distance metrics

## Installation

### Prerequisites

* Python 3.8 or higher
* Webcam/Camera on your device
* Windows, macOS, or Linux operating system

### Step-by-Step Setup

1. **Clone or Extract the Project**

   ```bash
   cd vigildrive_ai
   ```

2. **Create a Virtual Environment (Recommended)**

   ```bash
   # On Windows
   python -m venv venv
   venv\Scripts\activate

   # On macOS/Linux
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install Dependencies**

   ```bash
   pip install -r requirements.txt
   ```

4. **Verify Installation**

   ```bash
   python -c "import cv2, mediapipe, numpy, pygame; print('All dependencies installed successfully!')"
   ```

## How to Run

### Basic Usage

```bash
python main.py
```

The application will:

1. Open a window showing your webcam feed
2. Display facial landmarks and eye contours in green
3. Show real-time Eye Aspect Ratio (EAR) values
4. Track head nods and eye closure duration
5. Trigger an alarm when driver fatigue is detected

### Keyboard Controls

* **q**: Quit the application
* **r**: Reset the detector state and stop the alarm

## How It Works

### 1. Eye Aspect Ratio (EAR)

The system calculates the Eye Aspect Ratio using the formula:

```
EAR = (||p2 - p6|| + ||p3 - p5||) / (2 * ||p1 - p4||)
```

Where p1-p6 are the eye landmark coordinates. Lower EAR values indicate closed eyes.

* **Threshold**: EAR < 0.2 indicates closed eyes
* **Duration Check**: Eyes must stay closed for >2 seconds to trigger an alert

### 2. Head Nod Detection

The system tracks the vertical position of the nose tip (landmark index 1) to detect head nodding.

* **Movement Threshold**: >15 pixels vertical movement triggers a nod detection
* **Nod Count**: More than 3 nods within 10 seconds triggers an alert

### 3. Real-Time Processing

The system operates at approximately 30 FPS and:

1. Captures frames from the webcam
2. Detects facial landmarks using MediaPipe
3. Calculates EAR for both eyes
4. Tracks nod history within a time window
5. Determines driver alertness based on dual criteria
6. Triggers or stops the alarm accordingly

### 4. Alarm System

* Uses Pygame mixer for audio playback
* Can play a pre-recorded alarm sound (alarm_sound.wav)
* Automatically generates a 1kHz beep tone if no audio file is present
* Alarm loops continuously until the driver regains alertness

## Project File Structure

```text
vigildrive_ai/
├── main.py                 # Main application entry point
├── detector.py             # Driver alertness monitoring logic
├── alarm.py                # Alarm sound management
├── utils.py                # Helper functions and calculations
├── requirements.txt        # Python package dependencies
└── README.md               # Project documentation
```

### File Descriptions

* **main.py**: Contains the main application class that orchestrates video capture, detection, and alarm triggering
* **detector.py**: Implements the `DrowsinessDetector` class with eye and head movement analysis
* **alarm.py**: Implements the `DrowsinessAlarm` class for sound generation and playback
* **utils.py**: Contains utility functions for EAR calculation, coordinate normalization, and text rendering

## Performance Tuning

### Adjusting Sensitivity

You can modify the thresholds in `main.py` when initializing the detector:

```python
self.detector = DrowsinessDetector(
    ear_threshold=0.2,
    closed_eyes_duration_threshold=2.0,
    nod_count_threshold=3,
    nod_time_window=10.0,
)
```

### Common Adjustments

* **Too many false alarms**: Increase `ear_threshold` to 0.25 or increase `closed_eyes_duration_threshold` to 2.5
* **Not detecting fatigue**: Decrease `ear_threshold` to 0.15 or decrease `closed_eyes_duration_threshold` to 1.5
* **Nod detection too sensitive**: Increase `nod_count_threshold` to 4 or decrease `nod_threshold` in detector.py

## Troubleshooting

### Camera Not Detected

* Ensure your webcam is connected and functioning
* Try changing the camera index from 0 to 1 in `main.py`
* Check for permission issues (allow camera access in system settings)

### No Landmarks Detected

* Ensure adequate lighting
* Position your face clearly in the center of the frame
* Reduce distance from the camera (optimal: 30–60 cm)

### Alarm Not Playing

* Check system volume levels
* Ensure Pygame mixer initialized successfully
* Verify pygame installation:

  ```bash
  pip install --upgrade pygame
  ```

### Low FPS Performance

* Reduce frame resolution in `_initialize_camera()`
* Close background applications consuming CPU
* Try setting `static_image_mode=False` in detector.py for faster processing

## Limitations and Considerations

1. **Lighting Dependency**: Requires adequate lighting for accurate face detection
2. **Single Face Support**: Designed for one driver at a time
3. **Camera Angle Sensitivity**: Works best with a front-facing camera
4. **Real-Time Constraints**: Performance depends on CPU capabilities
5. **Personal Variations**: Optimal thresholds may vary between individuals

## Future Improvements

* [ ] Multi-face detection for passenger monitoring
* [ ] Machine learning classification for fatigue patterns
* [ ] Mobile and embedded device optimization using TensorFlow Lite
* [ ] Integration with vehicle telemetry systems
* [ ] Cloud-based analytics and reporting
* [ ] SMS/Email notification system
* [ ] Driver behavior analytics and safety scoring
* [ ] Customizable alarm sounds and visual alerts
* [ ] Data logging and statistical analysis
* [ ] Smartwatch and wearable device integration
* [ ] Deep learning-based eye state classification
* [ ] Facial expression and emotion detection
* [ ] Mobile phone usage detection using YOLOv8
* [ ] Seat belt detection using Computer Vision

## Methodology & References

### Eye Aspect Ratio

This project is based on the research paper:

**"Real-Time Eye Blink Detection using Facial Landmarks"**
by Tereza Soukupová and Jan Čech

### MediaPipe Face Mesh

* Uses Google's MediaPipe framework for efficient facial landmark detection
* Tracks 468 facial landmarks in real time
* Provides AI-powered face tracking and landmark extraction

## Authors

**Malleeshwar V**

Artificial Intelligence & Data Science

Academic Project – VigilDrive AI

## License

This project is provided for educational and research purposes.

## Contact & Support

Before reporting issues, please ensure:

1. All dependencies are correctly installed
2. Camera permissions are granted
3. Python version is 3.8 or higher
4. All files are in the correct directory structure

## Disclaimer

VigilDrive AI is designed for educational purposes and driver awareness. It should not be used as the sole safety mechanism in vehicles. Drivers must always follow traffic regulations, maintain adequate rest, and practice safe driving habits.
