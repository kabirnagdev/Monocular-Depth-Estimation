**Model** 

we using unet++ which is a variant of unet with denser connections 
input :  rgb image 
output model produces one output value per pixel representing pixel depth
and for the encoder part we are using resnext50_32x4d
![[Pasted image 20261021161125.png]]

## Epochs 0–1

Phase 1: Freeze the encoder

The ResNeXt-50 encoder's weights stay unchanged. The decoder learns to turn the extracted features into depth maps.

This protects the encoder from early, potentially unstable updates.

## Epochs 2–4

Phase 2: Fine-tune the entire model

The encoder is unfrozen. Both encoder and decoder weights can now adjust to the depth estimation task.
### How a U-Net becomes a depth estimator?

A U-Net is an encoder-decoder with skip connections. It was designed for segmentation, which means one prediction per pixel. Depth estimation is also one prediction per pixel. Only the target changes.

where unet loss function is on dice and cross entropy we are using **MSE(regression)**

**What Unet++ adds?**
1. reduce semantic gap between encoder and decoder features before it merges 
2. Cost is more parameters and more compute than normal unet

**Training setup**
optimizer : AdamW with onecycleLR 
metrics: SSIM

Visualization
take 16 samples from test dataset each sample contains an rgb image and its correspond
![[Pasted image 20261021163304.png]]

**Limitations and issues**

1. **Non-standard evaluation.** Depth estimation papers normally report AbsRel, RMSE (in metres), RMSE-log and the threshold accuracies δ<1.25, δ<1.25², δ<1.25³. SSIM and MSE on 8-bit normalized maps are not comparable to published NYUv2 results.
2. **Depth is quantized and normalized.** The Kaggle version stores depth as 8-bit PNGs, so you lose metric precision and the scale information. Your model learns something closer to _relative_ depth than metric depth.
3. **Augmentation artifacts.** `ShiftScaleRotate` with 45° rotation can introduce empty border regions. Those get a depth of 0 (“near”) in the target, which is not real data.
4. **Plain MSE loss.** Depth literature often uses scale-invariant log loss, gradient loss, or SSIM loss combined with L1, because pure MSE tends to give blurry depth maps. It is a cheap thing to ablate.
5. **Unbounded output.** The target is in [0, 1], but nothing constrains the output. You could try a sigmoid head.
6. **Small training budget.** Only 5 epochs, and the random splits are not tied to the official NYUv2 test set, so results are not benchmark-comparable.
7. **Test loader shuffles** and uses batch size 4. This is harmless, but it makes visual comparisons non-reproducible without a seed.