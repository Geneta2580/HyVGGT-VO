# <div align="center">HyVGGT-VO: Tightly Coupled Hybrid Dense Visual Odometry with Feed-Forward Models</div>

<p align="center">
  <a href="https://mac-vo.github.io"><img src="https://img.shields.io/badge/Homepage-4385f4?style=flat&logo=googlehome&logoColor=white"></a>
  <a href="https://arxiv.org/abs/2409.09479v2"><img src="https://img.shields.io/badge/arXiv-b31b1b?style=flat&logo=arxiv&logoColor=white"></a>
  <a href="./LICENSE"><img src="https://img.shields.io/badge/License-MIT-yellow.svg"></a>
</p>

<p align="center">
  <img src="asset/V101.gif" alt="EuRoC V101" width="40%" />
  <img src="asset/kitti_10.gif" alt="KTITTI 10" width="40%" />
</p>

## 📰 Updates
- **[2026-04-03]**: Our paper has been submitted to [arXiv](https://github.com/Geneta2580/gtsam.git).
- **[????-??-??]**: The full project source code will be released after our paper is accepted. If you are interested, please star ⭐ this repo to stay tuned!

---

## 💻 Project Configuration

### Hardware Requirements
This project was developed and tested in the following environment:
- **CPU**: AMD Ryzen 7940HX
- **GPU**: NVIDIA RTX 4060 Laptop (8GB VRAM)
- **OS**: Ubuntu 20.04

> **⚠️ Note**: To ensure the proper execution of the feed-forward networks and dense mapping, we highly recommend a device with at least **8GB of VRAM**.

### Environment Setup
We recommend using Anaconda for environment management.

1. **Create Base Environment**:
   ```bash
   conda create -n hyvggt python=3.10.12
   conda activate hyvggt
   ```

2. **Install PyTorch**:
   We recommend `PyTorch 2.3.1+cu121` as the base environment (please adjust according to your specific environment).

3. **Clone the Repository**:
   Since our project includes a custom VGGT submodule (which is not compatible with the official repository), please make sure to clone recursively:
   ```bash
   git clone --recursive https://github.com/Geneta2580/HyVGGT-VO.git
   cd HyVGGT-VO
   ```

4. **Install Dependencies & Custom VGGT**:
   ```bash
   pip install -r requirements.txt
   # Install the custom VGGT library
   pip install -e .
   ```

5. **Download Weights**:
   You need to download the VGGT weight files manually [\[VGGT model\]](https://huggingface.co/facebook/VGGT-1B/resolve/main/model.pt) and place them in the `submodules/vggt/` directory.

6. **GTSAM Configuration**:
   We use a customized version of the GTSAM library, which includes specific features for our framework and is **not** interchangeable with the official GTSAM repository.
   - Clone our custom GTSAM vio_inv_depth branch from: [\[Custom Branch\]](https://github.com/Geneta2580/gtsam.git)
   - The compilation process is identical to the official GTSAM guidelines. Please refer to: [\[GTSAM Guidelines\]](https://github.com/borglab/gtsam)

### Datasets Preparation
You can download the public datasets used for our evaluation from their official websites:
- **EuRoC MAV Dataset**: Can be obtained from the [ASL Datasets webpage](https://projects.asl.ethz.ch/datasets/euroc-mav/).
- **KITTI Odometry Dataset**: Can be obtained from the [CVLIBS webpage](https://www.cvlibs.net/datasets/kitti/).

---

## 🚀 Running the Code

**Dataset Evaluation Command**:
```bash
python3 main.py --config config/euroc/config_dataset_xxx.yaml > TEST.txt
```
- The execution logs will be redirected and saved to `TEST.txt`.
- **Parameters & Visualization**: You can toggle the visualization features and configure system parameters by modifying the corresponding `.yaml` file.
- **Data Loading**: Dataset reading is implemented via `dataloader.py`. Simply configure your dataset file path within the `yaml` file.

---

## 📊 Outputs

- **Trajectory Evaluation**: The system outputs trajectory files in `TUM` format (`.txt`), which can be directly evaluated against the ground truth using `evo`. The main trajectory outputs include:
  - `trajectory_odometry.txt`: The pose of every image frame output by the front-end.
  - `trajectory_optimized.txt`: The optimized poses of the keyframes.
  - `trajectory_vggt_optimized.txt`: The poses after incorporating local PGO (Pose Graph Optimization).
  > *Note: When evaluating on the KITTI dataset, the ground truth trajectory requires a UTC timestamp conversion before evaluation.*
- **Logs & Time Cost**: Essential running logs, including the computational time consumption (`time_cost`) and the trajectory of the VGGT inference subgraph, are automatically saved in the project's `/output/log/` directory.
- **Point Cloud**: Point cloud saving outputs can be configured in the `yaml` file, including the filtering conditions for output and downsampling parameters for visualization.

---

## 👁️ Visualization

- Our current visualization interface primarily supports **Open3D / OpenCV**.
- **⚠️ Performance Warning**: Enabling visualization will decrease the overall running speed of the system.
- **ROS 1 Support**: The ROS 1 version runs more efficiently and supports visualization via RViz. Please note that this version is currently under testing (beta). A stable release will be published in the future.

---

## 📂 Custom Datasets

Our framework supports custom datasets. To run your own data, please follow these guidelines:

1. **Required Information**: You must provide the camera calibration parameters (intrinsics and distortion coefficients) and accurate timestamps for the camera images.
2. **Code Modification**: Modify `dataloader.py` to ensure compatibility with your custom data format.
3. **ROS 1 Beta**: You can try our ROS 1 test version, which already supports custom camera dataset reading and receiving image data directly from camera ROS nodes.
4. **Best Practices**: For optimal computational efficiency, we recommend keeping the camera resolution **under 640x480** and the framerate **above 20 FPS**.

---

## 🙏 Acknowledgements

During the development of this code, we heavily referenced the outstanding implementations of the following open-source projects:
- [OV2SLAM](https://github.com/ov2slam/ov2slam)
- [VGGT-SLAM](https://github.com/MIT-SPARK/VGGT-SLAM)

We sincerely thank the authors for their great work!

---

## 📝 Citation / BibTex

If you find our work helpful for your research, please consider citing our paper:

```bibtex
@article{hyvggt_vo_2026,
  title={HyVGGT-VO: Tightly-Coupled Hybrid Dense Visual Odometry with Feed-Forward Models},
  author={Pang, Junxiang and [Other Authors...]},
  journal={arXiv preprint arXiv:XXXX.XXXXX},
  year={2026}
}
```