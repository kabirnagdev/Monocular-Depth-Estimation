[**Marigold V2**](https://arxiv.org/html/2609.08084v1)  sept 2026 
state-of-the-art framework that ==repurposes pretrained image-editing diffusion transformers (specifically **Qwen-Image-Edit-2509**) into dense geometric and intrinsic image estimators==. It produces exceptionally crisp, boundary-aware predictions for depth maps, see-through depth, surface normals, and albedo using a highly efficient **single-step inference** protocol
![[Pasted image 20261022084754.png]]
Quantitative Performance

- **KITTI & ETH3D:** Achieves a **16% to 26% improvement in Absolute Relative Error (AbsRel)** over previous best methods.

- **Zero-Shot Generalization:** Delivers state-of-the-art results on **NYUv2, ScanNet, and DIODE** without task-specific retraining.

- **AbsRel Specifics:** Reaches record-low error rates, down to an AbsRel of **5.4 on KITTI** and **2.8 on ETH3D**

[**Depth Anything 3**](https://seed.bytedance.com/en/blog/seed-research-depth-anything-3-a-single-transformer-architecture-for-space-reconstruction-from-any-views) Nov 2025

Core Architecture
- **Backbone:** Uses a single plain vision transformer (such as a vanilla DINOv2 encoder) without task-specific engineering. 
- **Unified Representation:** Predicts a coupled **depth-ray representation** (an exponential depth map and a 6D per-pixel ray map containing ray origin and direction). 
- **Dual-DPT Head:** Shares reassembly modules and branches only at the final fusion stage to jointly decode depth and rays. 
- **Cross-View Attention:** Employs input-adaptive cross-view self-attention in select transformer layers to process arbitrary numbers of views efficiently. 
![[Pasted image 20261022085135.png]]

Benchmarks
- **Camera Pose Accuracy:** Surpasses prior state-of-the-art models like VGGT by **35.7% to 44.3%** on average.
- **Geometric Reconstruction Accuracy:** Surpasses prior models by **23.6% to 25.1%** in surface and structural consistency.
- **Monocular Depth:** Outperforms its predecessor, _Depth Anything 2_, (e.g., >10% improvement on ETH3D).
- **Evaluation Metrics:** Uses RANSAC-based pose alignment and TSDF (Truncated Signed Distance Function) fusion for direct 3D consistency, measuring Area Under the Curve (AUC) for pose and geometry

[**MOGE 2**](https://arxiv.org/pdf/2507.02546) July 2025

![[Pasted image 20261022085708.png]]
benchmarks
![[Pasted image 20261022085758.png]]Architecture & Implementation Methods
- **Decoupled Representation:** The model builds upon the original affine-invariant MoGe by separating **relative geometry** (shape) from **global metric scale**. 
- **Backbone & Decoder:** Uses a **DINOv2** image encoder paired with dedicated convolutional and MLP decoders. 
- **Scale Head:** An MLP conditioned on **DINOv2 CLS tokens** predicts the absolute metric scale, allowing the global scale head to run during inference without needing scale-head-off modes. 
- **Unified Data Refinement Pipeline:**
    - _Filtering:_ Removes noisy, misaligned, or incorrect real-world RGB-D boundaries using multi-scale local alignment against a synthetic-trained model (\(G_{\text{syn}}\)).
    - _Completion:_ Fills missing or sparse regions using edge-preserving logarithmic-space Poisson depth completion guided by sharp synthetic labels. 
- **Inference:** Consumes the dense depth/point-map pathway from frozen checkpoints (such as `moge-2-vitl`), with evaluation pipelines available via the official


[Dino v2](https://dinov2.metademolab.com/)


[DiT Diffuser](https://encord.com/blog/diffusion-models-with-transformers/)
A **DiT (Diffusion Transformer)** in machine learning is a generative model that replaces the traditional U-Net backbone with a **Transformer architecture**
![[Pasted image 20261022090428.png]]

![[Pasted image 20261022090756.png]]

relative depth zero shot accuracy![[Pasted image 20261022090830.png]]