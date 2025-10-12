# DriverDrowsinessDetection
This Python project detects driver drowsiness in real time using eye aspect ratio (EAR) from facial landmarks. If eyes stay closed beyond a threshold, it triggers an alert sound. Built with OpenCV, dlib, and pygame, it enhances road safety by monitoring driver alertness via webcam.
## 🔍 Features
- Real-time video monitoring
- Eye Aspect Ratio (EAR) calculation
- Facial landmark detection using dlib
- Audio alert when drowsiness is detected
- Visual feedback with eye contours and warning messages

## 🛠️ Technologies Used
- Python
- OpenCV
- dlib
- scipy
- pygame
- imutils

## 🚀 Getting Started

### 1. Prerequisites

Make sure you have Python installed on your system. You will also need to install the required libraries.

### 2. Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/your-username/DriverDrowsinessDetection.git
    cd DriverDrowsinessDetection
    ```

2.  **Install dependencies:**
    Create a `requirements.txt` file with the following content:
    ```
    opencv-python
    dlib
    scipy
    pygame
    imutils
    ```
    Then, run the following command to install the required packages:
    ```bash
    pip install -r requirements.txt
    ```
    *(Note: If you encounter issues installing `dlib`, you may need to install `CMake` first.)*

3.  **Download the Dlib Model:**
    The script requires a pre-trained facial landmark model. Create a `models` directory and download the file into it by running:
    ```bash
    mkdir models
    wget http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2 -P models/
    bzip2 -d models/shape_predictor_68_face_landmarks.dat.bz2
    ```

### 3. Running the Detector

Make sure your webcam is connected and not in use by another application. Run the script from the project's root directory:

```bash
python Drowsiness_Detection.py
```

A window will appear showing your webcam feed, with green contours around your eyes and the real-time Eye Aspect Ratio (EAR) displayed in the top-right corner.

To exit, ensure the video window is active and press the **'q'** key.

## 🎯 How to Calibrate and Check Accuracy

The "accuracy" of this system depends on how well it is tuned for a specific user and environment.

### Real-time Calibration

The two most important parameters to tune are `thresh` and `frame_check` in the `Drowsiness_Detection.py` script.

*   **Calibrating `thresh` (EAR Threshold):**
    1.  Run the script and observe the `EAR:` values printed in your terminal.
    2.  Note the typical EAR value when your eyes are open.
    3.  Note the EAR value when you blink.
    4.  The ideal `thresh` should be between your "open-eye" and "blinking" EAR values. The default of `0.25` is a good starting point, but you may need to adjust it.
    5.  Update the `thresh = 0.25` line in the script with your calibrated value.

*   **Calibrating `frame_check`:**
    1.  This parameter sets the number of consecutive frames your eyes must be closed for the alarm to trigger.
    2.  If the alarm is too sensitive (e.g., triggers on long blinks), **increase** this value (e.g., to `25` or `30`).
    3.  If the alarm is too slow to react, **decrease** it (e.g., to `15`).

### Formal Accuracy Checking

For a more systematic evaluation of the model's performance, follow these steps:

1.  **Create a Labeled Dataset:**
    *   Record short video clips under realistic conditions (e.g., different people, lighting).
    *   Label each clip with a "ground truth": either **"drowsy"** or **"alert."**

2.  **Run the Model on Your Dataset:**
    *   Modify the script to process video files instead of the live webcam. Change `cv2.VideoCapture(0)` to `cv2.VideoCapture('path/to/your/video.mp4')`.
    *   Run the script on each video and log whether it triggered an "ALERT!" or not.

3.  **Evaluate Performance:**
    *   Compare the script's predictions to your ground truth labels. This will give you:
        *   **True Positives (TP):** Correctly predicted "drowsy."
        *   **True Negatives (TN):** Correctly predicted "alert."
        *   **False Positives (FP):** Incorrectly predicted "drowsy" (false alarm).
        *   **False Negatives (FN):** Incorrectly predicted "alert" (missed detection).

4.  **Calculate Metrics:**
    *   Use these counts to calculate standard performance metrics:
        *   **Accuracy:** `(TP + TN) / (Total Videos)`
        *   **Precision:** `TP / (TP + FP)` (Measures false alarm rate)
        *   **Recall:** `TP / (TP + FN)` (Measures missed detection rate)
