# 🧠 Football Player Tracking using YOLOv8 + ByteTrack

This project performs object detection and tracking on a football match video using a custom YOLOv8 model and ByteTrack. The goal is to track **players**, **referees**, and **goalkeepers**, while **ignoring the ball**.

---

## 📁 Folder Structure

📦 project-root/
┣ 📜 best.pt # Your trained YOLOv8 model
┣ 📜 15sec_input_720p.mp4 # Input video
┣ 📁 ByteTrack/ # Cloned ByteTrack repo (automated in setup)
┣ 📁 runs/track/tracked_output/ # Output folder with tracked .avi and .mp4 files
┣ 📜 README.md

yaml
Copy
Edit

---

## 🚀 Setup Instructions (Google Colab Recommended)

### 🔧 Step 1: Install Required Libraries

```bash
!pip install ultralytics --quiet
!pip install -U gdown
!gdown --id 1-5fOSHOSB9UXyP_enOoZNAMScrePVcMD --output best.pt
📦 Step 2: Clone ByteTrack and Install Dependencies
bash
Copy
Edit
!git clone https://github.com/ifzhang/ByteTrack.git
%cd ByteTrack
!pip install -r requirements.txt
!pip install cython lap
!python setup.py develop
🧠 Run Tracking (with YOLOv8 + ByteTrack)
python
Copy
Edit
from ultralytics import YOLO

# Load your trained YOLOv8 model
model = YOLO('/content/best.pt')

# Track players, referees, and goalkeepers (excluding ball)
model.track(
    source='/content/15sec_input_720p.mp4',  # Input video
    conf=0.4,
    iou=0.5,
    tracker='bytetrack.yaml',
    classes=[1, 2, 3],  # Ignore class 0 (ball)
    save=True,
    project='/content',
    name='tracked_output'
)
🎬 Convert Tracked AVI to MP4 (Optional)
bash
Copy
Edit
!ffmpeg -i /content/tracked_output/15sec_input_720p.avi \
        -c:v libx264 -preset fast -crf 22 \
        /content/tracked_output/15sec_input_720p.mp4
📌 Requirements
Python 3.8+

OpenCV

Ultralytics YOLOv8 (ultralytics)

ByteTrack (cloned from GitHub)

Cython, NumPy, lap

ffmpeg (for video conversion)

✅ Output
The final video is saved to:

bash
Copy
Edit
/content/tracked_output/15sec_input_720p.mp4
It contains bounding boxes and IDs for each tracked player (referees, goalkeepers, outfield players).

📝 Notes
Make sure your best.pt is trained on correct classes:
0: ball, 1: goalkeeper, 2: player, 3: referee

This setup is optimized for Google Colab.

Evaluation metrics like MOTA/MOTP require ground truth annotations in MOT format (not included here).

✍️ Author
Developed by Priyanshu Singh
B.Tech in Electronics & Communication Engineering
📧 priyanshu.asn2003@gmail.com
