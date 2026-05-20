# From Video to 3D: Intelligent Furniture Placement & Vastu-Based Room Design

## Project Overview
An end-to-end AI system that takes a short room video as input, 
reconstructs the room in 3D, detects existing furniture, and 
recommends optimized furniture placement based on Vastu Shastra 
principles using a Genetic Algorithm.

## System Architecture
The system operates in two phases:

**Phase 1 — 3D Reconstruction Pipeline**
- Panoramic RGB images from the Structured3D dataset
- MiDaS DPT Hybrid depth estimation
- U-Net + Vision Transformer (ViT) semantic segmentation
- 3D point cloud generation (514,352 points)
- Poisson mesh reconstruction (~259,792 vertices)

**Phase 2 — Vastu Optimization Pipeline**
- User uploads a short room video (mp4/mov/avi)
- Frame extraction using OpenCV
- YOLOv8n furniture detection (15 object categories)
- MiDaS depth estimation → 3D coordinate extraction
- Vastu zone assignment on a 3×3 directional grid
- Genetic Algorithm optimization (50 population, 100 generations)
- 3D visualization + downloadable Vastu compliance report

---

## Key Results

| Component | Metric | Result |
|---|---|---|
| U-Net Segmentation | Pixel Accuracy | 94.6% |
| U-Net Segmentation | Mean IoU | 32.8% |
| ViT Multimodal | Pixel Accuracy | 75.8% |
| 3D Point Cloud | Points Generated | 514,352 |
| Vastu GA Optimization | Initial Vastu Score | 50.0% |
| Vastu GA Optimization | Final Vastu Score | 100.0% |
| Vastu GA Optimization | Violations Removed | 3 of 3 (100%) |

---

## Tech Stack

| Area | Technologies |
|---|---|
| Deep Learning | PyTorch, U-Net, Vision Transformer (ViT) |
| Computer Vision | OpenCV, YOLOv8n (COCO), MiDaS DPT |
| 3D Reconstruction | Open3D, Poisson Mesh, Point Cloud |
| Optimization | Genetic Algorithm (custom implementation) |
| Backend | Django REST Framework, Flask |
| Visualization | Three.js, Matplotlib, Trimesh |
| Dataset | Structured3D (100 scenes, 2,016 samples) |
| Language | Python |

---

## Models Used
- **MiDaS DPT Hybrid** — monocular depth estimation
- **U-Net** (13.4M parameters) — RGB+Depth semantic segmentation
  - Input: 4-channel (RGB + Depth), 256×256
  - Output: 211 semantic classes
  - Training: 30 epochs, Adam optimizer, lr=1e-3
- **Vision Transformer ViT Multimodal** (45.6M parameters)
  - Patch size: 16×16, 256 patches, embedding dim: 768
  - 8 attention heads, 6 encoder layers
- **YOLOv8n** — furniture object detection (inference only)
- **Genetic Algorithm** — Vastu-based furniture placement
  - Population: 50, Generations: 100
  - Fitness = Vastu penalty + Movement penalty + Overlap penalty

---

## Vastu Intelligence
The system implements Vastu Shastra rules for 11 furniture 
categories mapped to a 3×3 directional zone grid:

- **North-East (Ishanya):** Water energy — sink, plants, TV
- **South-East (Agni):** Fire energy — oven, refrigerator, microwave
- **South-West (Nairutya):** Earth/stability — bed, heavy storage
- **North:** Wealth energy — desk, laptop, books

The Genetic Algorithm minimizes:
- Vastu violations (+10 penalty per violation)
- Furniture movement (0.2 × Euclidean distance)
- Object overlap (+5 per collision pair)

---

## System Outputs
- Rendered PNG: before/after Vastu compliance layout
- 3D Model: `.glb` and `.obj` files (viewable in Blender/browser)
- Point Cloud: `.ply` file
- JSON Report: detected objects, zones, compliance, GA fitness
- Vastu Summary: score, violations, per-object recommendations

---

## Dataset
- **Structured3D** — 100 scenes, 14,112 panoramic images
- Used: 2,016 matched RGB-Depth-Label triplets
- Split: 80% train / 10% validation / 10% test

---

## How to Run
```bash
# Clone the repository
git clone https://github.com/Sajita01/From-Video-to-3D-room-design

# Install dependencies
pip install -r requirements.txt

# Run Phase 1 — 3D Reconstruction
python reconstruction_pipeline.py

# Run Phase 2 — Vastu Optimization
python vastu_pipeline.py --video your_room_video.mp4
```

---

## Limitations & Future Work
- MiDaS produces relative depth (not metric) — 
  LiDAR integration planned for future
- YOLOv8n trained on COCO — 
  fine-tuning on Nepali furniture types is a future goal
- Real-time AR overlay on mobile is a planned enhancement
- Full Structured3D dataset training (196,000 images) 
  would improve segmentation accuracy

