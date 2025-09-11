# Video Processing Pipeline

Main Processing Script: scripts/convert_vdo_to_skeletons.py:17

- Processes .mp4 and .MP4 video files
- Uses MediaPipe models for face, hand, and pose landmark detection
- Combines landmarks from all body parts into unified skeleton data

Processing Steps

1. Landmark Extraction (scripts/convert_vdo_to_skeletons.py:73-84):

   - Face landmarks via FaceLandmarker
   - Pose landmarks via PoseLandmarker
   - Hand landmarks via HandLandmarker with pose-based approximation

2. Data Combination (scripts/convert_vdo_to_skeletons.py:87):

   - Concatenates face, hand, and pose landmarks into single array
   - Shape: [frames, total_landmarks, 3] (x,y,z coordinates)

Output Files Generated

Primary Outputs (per video):

1. .npy files (scripts/convert_vdo_to_skeletons.py:105):

   - Raw landmark data in NumPy format
   - Contains normalized or pixel coordinates based on config

2. .txt files (statistics) (scripts/convert_vdo_to_skeletons.py:182-185):

   - Face landmark statistics
   - Hand landmark statistics
   - Pose landmark statistics

3. .mp4 files (optional) (scripts/convert_vdo_to_skeletons.py:115-171):

   - Visualized landmark overlay videos
   - Shows original + landmarks + white background versions

Joint-Based Conversion Outputs:

Script: scripts/convert_pos_to_rel_angle_hand.py

4. .skels files (scripts/convert_pos_to_rel_angle_hand.py:330):

   - Hand joint data converted to relative angles
   - Normalized to [-1, 1] range
   - Contains 42 joints (21 per hand) with 3D angle data

5. Global normalization files:

   - global_min_rel_angle.npy (scripts/convert_pos_to_rel_angle_hand.py:197)
   - global_max_rel_angle.npy (scripts/convert_pos_to_rel_angle_hand.py:198)
   - global_min_bl.npy (scripts/convert_pos_to_rel_angle_hand.py:199)
   - global_max_bl.npy (scripts/convert_pos_to_rel_angle_hand.py:200)

   Format for Blender 3D Rendering:

Based on the rendering pipeline in your codebase:

Required Input Format:

File: src/T2M_GPT/visualize/render_mesh.py:195-201

- Format: .npy files with motion data
- Expected Structure:
  {
  "motion": [frames, joints, features], # Joint positions/rotations
  "lengths": [sequence_lengths],
  "text": [descriptions]
  }

For Blender Rendering:

1. Use .npy files (not .skels) as primary input
2. Convert through SMPL pipeline (src/T2M_GPT/visualize/vis_utils.py:28-31):


    - Joint positions → SMPL parameters → 3D mesh vertices

3. Output formats available:


    - .obj files per frame (src/T2M_GPT/render_final.py:31)
    - .npy with SMPL parameters (src/T2M_GPT/render_final.py:34)
    - Direct mesh vertices for Blender import

Recommended Pipeline for Blender:

1. Start with .npy landmark files
2. Use render_final.py to generate .obj files
3. Import .obj sequence into Blender for animation
4. Or use SMPL parameters .npy with Blender SMPL addon

The .npy format contains the 3D positional data needed for mesh generation, while .skels contains processed angle data better suited for motion
analysis than 3D rendering.
