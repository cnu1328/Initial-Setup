# Initial-Setup
 This repository contains all the basic setups for various technologies. It also includes some code snippets that might be helpful in different scenarios.

### **Comparison of Object Detection Frameworks for Drone Video Analysis**  

| **Framework**      | **Best Features** | **Limitations** | **Suitability for Drone Video Analysis** |
|--------------------|------------------|-----------------|------------------------------------------|
| **OpenCV** | 1. Used for frame extraction and basic image processing. <br> 2. Efficient for pre-processing tasks like resizing, filtering, and motion detection. <br> 3. Lightweight and easy to use. | 1. Not an object detection model. <br> 2. Requires integration with another model (e.g., YOLO) for actual object detection. | Essential for pre-processing but not for detection. Should be used alongside other models. |
| **YOLOv8 (Ultralytics)** | 1. Real-time detection with high speed and accuracy. <br> 2. Lightweight and optimized for edge devices. <br> 3. Works well with small objects like humans and vehicles in drone footage. <br> 4. Supports object tracking (DeepSORT integration). | 1. Not specifically designed for anomaly detection. <br> 2. Might need fine-tuning with a custom dataset for best results in security analysis. | Best choice for real-time object detection in drone footage. Can be combined with tracking models for movement analysis. Great for detecting security threats and intrusions. |
| **MMDetection** | 1. Highly modular & customizable for complex object detection tasks. <br> 2. Based on PyTorch, allowing deep customization. <br> 3. State-of-the-art detection models, including Cascade R-CNN, Faster R-CNN. | 1. Slower than YOLO for real-time applications. <br> 2. Requires more computational power. <br> 3. Not as easy to integrate as YOLOv8. | Overkill for simple object detection in drone footage. Useful if you need advanced anomaly detection via custom models. |
| **Detectron2 (Meta AI)** | 1. Very powerful deep-learning-based object detection framework. <br> 2. Supports mask detection, segmentation, and pose estimation. <br> 3. Highly accurate on complex datasets. | 1. Heavyweight framework, requires high-end GPUs. <br> 2. More difficult to deploy compared to YOLOv8. <br> 3. Not optimized for real-time drone footage analysis. | Too complex for simple security analysis in drone videos. Suitable for advanced surveillance applications where segmentation is needed. |
| **Mediapipe** | 1. Best for pose estimation and hand tracking. <br> 2. Lightweight and runs efficiently on edge devices. <br> 3. Works well for tracking human movements. | 1. Not an object detection model. <br> 2. Limited to pose tracking and gesture recognition. | Not useful for general security detection in drone footage. Can be helpful if you need detailed human pose tracking (e.g., detecting people climbing fences, running, etc.). |

### **Final Choice: YOLOv8 + OpenCV + DeepSORT**
- **YOLOv8** → Best for real-time object detection.  
- **OpenCV** → Helps in video pre-processing & frame extraction. 


### **Comparison of Video Action Recognition Frameworks for Drone Video Analysis**  

| **Framework** | **Best Features** | **Limitations** | **Suitability for Drone Video Analysis** |
|--------------|------------------|-----------------|------------------------------------------|
| **SlowFast (Facebook AI)** | 1. High accuracy in action recognition by combining fast (motion) and slow (context) pathways. <br> 2. Effective for detecting both rapid movements and long-term activities. <br> 3. State-of-the-art performance on video understanding tasks. | 1. Computationally expensive, requiring high-end GPUs. <br> 2. Slower inference speed compared to lightweight models. | Best for high-accuracy security analysis where movement patterns need to be analyzed in detail. |
| **TSM (Temporal Shift Module)** | 1. Efficient and lightweight compared to SlowFast. <br> 2. Works well on edge devices and real-time applications. <br> 3. Suitable for recognizing short-term motion patterns. | 1. Less effective in capturing long-term dependencies in videos. <br> 2. Requires additional post-processing for complex activities. | Good choice for real-time drone video analysis when efficiency is a priority. |
| **I3D (Inflated 3D Networks)** | 1. Uses 3D convolutions for better spatiotemporal feature extraction. <br> 2. Pre-trained models available for action recognition tasks. <br> 3. Strong baseline for understanding human activities. | 1. Computationally expensive and requires significant resources. <br> 2. Not optimized for real-time processing on web applications. | Suitable for offline analysis where detailed action recognition is needed but not ideal for real-time applications. |
| **TimeSformer** | 1. Transformer-based architecture for video processing. <br> 2. Captures long-range dependencies efficiently. <br> 3. Provides high accuracy in complex video action recognition tasks. | 1. Requires significant computational power, making real-time processing challenging. <br> 2. Not as optimized for short-term motion analysis. | Best for detailed post-processing security analysis but not for real-time drone surveillance. |

### **Final Choice: TSM (Temporal Shift Module) + YOLOv8**
- **TSM** → Efficient and lightweight for recognizing security-related actions (e.g., running, climbing, fighting).  
- **YOLOv8** → Best for detecting objects and threats in drone footage.  

This combination balances speed, accuracy, and computational efficiency for a real-time security detection system.



### **Comparison of Anomaly Detection Models for Drone Surveillance**  

| **Model** | **Best Features** | **Limitations** | **Suitability for Drone Video Analysis** |
|-----------|------------------|-----------------|------------------------------------------|
| **MOG2 (Mixture of Gaussians) – OpenCV-based** | 1. Fast and lightweight background subtraction model. <br> 2. Good for detecting moving objects (intrusions, suspicious activity). <br> 3. Works well in low-resource environments. | 1. Not deep-learning-based, so it struggles with complex scene understanding. <br> 2. High false positives in dynamic environments (e.g., moving trees, shadows). | ✅ Useful for **basic motion-based anomaly detection**, but lacks deep context understanding. |
| **ConvLSTM (Convolutional LSTM - PyTorch-based)** | 1. Captures both spatial (frames) and temporal (time) information. <br> 2. Good for detecting unusual movement patterns. <br> 3. Can handle sequential data well. | 1. Requires large training data to generalize well. <br> 2. Computationally expensive compared to traditional models. | ✅ Best for **detecting anomalies based on movement patterns** in drone footage. |
| **Autoencoders for Anomaly Detection** | 1. Learns normal behavior and detects deviations (e.g., suspicious activity). <br> 2. Works well for unsupervised anomaly detection. <br> 3. Can detect unknown threats without labeled data. | 1. Needs high-quality normal samples for training. <br> 2. May not detect short-duration anomalies effectively. | ✅ Good for **detecting unknown or rare security threats**. |
| **GANs (Generative Adversarial Networks) for Anomaly Detection** | 1. Learns normal patterns and flags outliers effectively. <br> 2. Works well for detecting unseen anomalies. <br> 3. Can generate synthetic training data for better performance. | 1. Hard to train and fine-tune. <br> 2. Computationally expensive. | ❌ Overkill for simple security anomaly detection but **useful for high-security applications**. |
| **VQ-VAE (Vector Quantized Variational Autoencoder)** | 1. Captures both spatial and temporal features for anomaly detection. <br> 2. Can compress video data efficiently. <br> 3. Useful for detecting structural anomalies. | 1. Computationally heavy. <br> 2. Requires large-scale training datasets. | ❌ Too complex for real-time drone surveillance but **useful for offline analysis**. |
| **Variational Autoencoders (VAEs)** | 1. Learns complex patterns and identifies anomalies based on probability distribution shifts. <br> 2. Works well in semi-supervised settings. <br> 3. Can handle high-dimensional video data. | 1. Hard to fine-tune for real-time processing. <br> 2. Requires high GPU resources for training. | ❌ Good for research but **not practical for real-time surveillance**. |

### **Best Choice for This Project**
#### ✅ **ConvLSTM + Autoencoders**
1. **ConvLSTM** → Detects unusual movement patterns (e.g., a person running unexpectedly, unusual vehicle movements).  
2. **Autoencoders** → Detect anomalies **without requiring labeled data** (e.g., unauthorized entry, unknown activities).  

💡 **How to Use These in the Project**:
- Use **YOLOv8** for object detection.  
- Use **ConvLSTM** to track movement patterns over time.  
- Use **Autoencoders** to detect unexpected behaviors (e.g., a vehicle stopping in a restricted zone).  

This combination ensures **efficient, real-time anomaly detection** with **minimal false positives**.



### Sample code to understand how it works

Here’s how you can integrate these tools step by step, along with a flowchart and explanation.

---

## **1. Understanding the Flow**  
When a user uploads a video, the system goes through multiple steps:

1. **Frame Extraction (FFmpeg & OpenCV)**
   - Extracts frames from the video at intervals.
   - Generates timestamps for each frame.

2. **Object Detection (YOLOv8)**
   - Identifies objects such as people, vehicles, and suspicious items.
   - Outputs bounding boxes, labels, and timestamps.

3. **Activity Recognition (TSM)**
   - Analyzes movement sequences to classify actions.
   - Detects normal vs. suspicious behaviors.

4. **Anomaly Detection (ConvLSTM)**
   - Detects unusual movement patterns over time.
   - Flags anomalies like trespassing or crowd formation.

5. **Report Generation (GPT-4)**
   - Compiles detected events into a structured summary.
   - Provides security insights and recommendations.

---

## **2. Flowchart Representation**
```plaintext
    User Uploads Video
           │
           ▼
    Frame Extraction (FFmpeg, OpenCV)
           │
           ▼
    Object Detection (YOLOv8)
           │
           ▼
    Activity Recognition (TSM)
           │
           ▼
    Anomaly Detection (ConvLSTM)
           │
           ▼
    Report Generation (GPT-4)
           │
           ▼
    User Receives Security Report
```

---

## **3. Sample Code for Integration**
Here's a simplified Python script to understand the integration of these components.

### **Step 1: Frame Extraction using OpenCV**
```python
import cv2
import os

def extract_frames(video_path, output_folder, fps=5):
    os.makedirs(output_folder, exist_ok=True)
    cap = cv2.VideoCapture(video_path)
    frame_rate = cap.get(cv2.CAP_PROP_FPS)
    frame_interval = int(frame_rate / fps)
    
    frame_count = 0
    extracted_count = 0

    while cap.isOpened():
        ret, frame = cap.read()
        if not ret:
            break
        
        if frame_count % frame_interval == 0:
            timestamp = cap.get(cv2.CAP_PROP_POS_MSEC) / 1000  # Convert to seconds
            frame_filename = f"{output_folder}/frame_{extracted_count:04d}_{timestamp:.2f}.jpg"
            cv2.imwrite(frame_filename, frame)
            extracted_count += 1
        
        frame_count += 1

    cap.release()
    print(f"Extracted {extracted_count} frames.")
```

**Example Output:**
```
Extracted 50 frames.
Saved frame_0000_0.00.jpg
Saved frame_0001_0.20.jpg
Saved frame_0002_0.40.jpg
...
```

---

### **Step 2: Object Detection using YOLOv8**
```python
from ultralytics import YOLO
import glob

model = YOLO("yolov8n.pt")  # Load pre-trained YOLOv8 model

def detect_objects(image_folder):
    image_files = glob.glob(f"{image_folder}/*.jpg")
    results = {}

    for img in image_files:
        detections = model(img)  # Run YOLO on each frame
        results[img] = detections.pandas().xyxy[0]  # Store results
    
    return results
```

**Example Output:**
```
frame_0000_0.00.jpg → Person detected at (x1:100, y1:150, x2:200, y2:300)
frame_0001_0.20.jpg → Vehicle detected at (x1:50, y1:100, x2:250, y2:400)
```

---

### **Step 3: Activity Recognition using TSM**
```python
from tsm_model import TSM  # Assume we have a trained TSM model

tsm_model = TSM()

def classify_activities(frames):
    sequences = group_frames_into_sequences(frames)  # Prepare input for TSM
    activity_labels = tsm_model.predict(sequences)
    
    return activity_labels
```

**Example Output:**
```
0:15 → Person walking normally
0:30 → Person running (Suspicious)
```

---

### **Step 4: Anomaly Detection using ConvLSTM**
```python
from convlstm_model import ConvLSTM  # Assume we have a trained ConvLSTM model

anomaly_detector = ConvLSTM()

def detect_anomalies(frame_sequences):
    anomalies = anomaly_detector.predict(frame_sequences)
    return anomalies
```

**Example Output:**
```
1:00 → Anomaly detected: Person standing near restricted area for too long
```

---

To enhance your function for generating comprehensive video summaries, we can modify the `generate_report` function to accept detailed descriptions of detected objects, activities, and anomalies, and then utilize OpenAI's GPT-4 to produce a structured narrative of the video's events. Below is the revised function along with a sample usage and its output.

**Revised Function:**

```python
import openai

openai.api_key = "YOUR_OPENAI_API_KEY"

def generate_video_summary(detected_objects, activities, anomalies):
    """
    Generates a comprehensive video summary based on detected objects, activities, and anomalies.

    Parameters:
    - detected_objects (dict): Dictionary with timestamps as keys and descriptions of detected objects as values.
    - activities (dict): Dictionary with timestamps as keys and descriptions of activities as values.
    - anomalies (dict): Dictionary with timestamps as keys and descriptions of anomalies as values.

    Returns:
    - str: A structured narrative summarizing the video's events.
    """
    prompt = f"""
    You are an AI assistant tasked with summarizing the events of a surveillance video based on the following detections:

    Detected Objects:
    {detected_objects}

    Activities:
    {activities}

    Anomalies:
    {anomalies}

    Please provide a detailed and structured narrative of the events that occurred in the video, mentioning specific timestamps and describing the sequence of actions and any potential security concerns.
    """
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}]
    )
    return response["choices"][0]["message"]["content"]

# Example Usage
detected_objects = {
    "00:15": "Person detected near the main entrance",
    "00:30": "Suspicious vehicle detected approaching the building",
    "00:45": "Unattended bag detected in the lobby area"
}

activities = {
    "00:20": "Person entering the building",
    "00:35": "Vehicle parking near the loading dock",
    "00:50": "Person leaving the bag and walking away"
}

anomalies = {
    "00:55": "Person loitering in the restricted area",
    "01:10": "Vehicle making repeated passes around the building"
}

report = generate_video_summary(detected_objects, activities, anomalies)
print(report)
```
**Output:**

```
At 00:15, a person was detected near the main entrance.  
At 00:20, the person entered the building.  
At 00:30, a suspicious vehicle was detected approaching the building.  
At 00:35, the vehicle parked near the loading dock.  
At 00:45, an unattended bag was detected in the lobby area.  
At 00:50, the person who left the bag was seen walking away.  
At 00:55, the same person was observed loitering in a restricted area.  
At 01:10, the vehicle was seen making repeated passes around the building.  

These events suggest potential security concerns that may require further investigation.  
```

