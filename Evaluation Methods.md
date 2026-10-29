## 1. DenseDepth – High Quality Monocular Depth Estimation via Transfer Learning
It uses **transfer learning** rather than training the entire network from scratch.

- **Encoder:** A pretrained **DenseNet-169** extracts visual features.
- **Decoder:** Upsamples those features to recover spatial information and produce a dense depth map.
- **Skip connections:** Transfer features from earlier encoder layers to the decoder, helping preserve details such as edges and object boundaries.
- **Output:** One depth value for essentially every pixel.
   ![[Pasted image 20260926012209.png]]

## 2. MiDaS v3.1 – A Model Zoo for Robust Monocular Relative Depth Estimation

Instead of training only on one dataset and expecting the model to work everywhere, MiDaS combines information from multiple depth datasets and uses training strategies that help the model generalize to images it hasn't seen before.
![[Pasted image 20260926012407.png]]

## 3. ZoeDepth – Zero-shot Transfer by Combining Relative and Metric Depth

It builds on the **DPT/MiDaS family of relative-depth models** and adds a **metric-depth head**.

One of its important ideas is **bin-based metric depth estimation**.

Instead of directly predicting a continuous depth value for every pixel, the model learns depth ranges/bins and estimates where each pixel falls within those ranges.

### Why "Zero-shot Transfer"?

This is one of the most important parts of the paper.

The model is designed to transfer knowledge from relative-depth estimation to **metric depth estimation across different datasets**.

![[Pasted image 20260926012518.png]]

## 4. DPT – Dense Prediction Transformer
A depth model needs to understand the relationship between these regions, not just the local pixels around each point.

DPT uses Transformer representations to capture these relationships and then reconstructs a high-resolution dense prediction.

The interesting part is that DPT doesn't simply take the final Transformer representation.

It extracts representations from **different stages of the Transformer** and combines them in a decoder.

This helps preserve both:

- **Global information** — overall scene structure
- **Local information** — object boundaries and fine details


![[Pasted image 20260926012746.png]]