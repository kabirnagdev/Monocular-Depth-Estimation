**Training**

In Stage 1,
we adapt the pretrained image-editing DiT to monocular depth estimation using latent rectified-flow supervision together with pixel-space and semantic feature losses. This stage establishes a strong affine-invariant depth predictor with accurate global structure and improved local reconstruction quality. 
In Stage 2,
we further fine-tune the resulting model using the proposed **SinkLoss**. This second stage is designed to refine fine details and reduce the ef fect of ambiguous or noisy ground-truth pixels, especially around transparent, thin, or indiscernible structures.
![[Pasted image 20261029084121.png]]

**Diffuser Transform adaptation**
ameter efficient fine-tuning. To make training memory-efficient, we apply 4-bit quantization to the pretrained DiT weights and fine-tune rank 128 QLoRA adapters. Following the Lotus-2 rectified-flow formu lation, we train the model to directly transform RGB latents into grayscale depth latents using a single forward pass. Let Evae and Dvae denote the pretrainedVAE encoder and decoder. We encode the RGB image 𝐼 and normalized depth target

𝑑 as 𝑧𝐼 = Evae(𝐼), 𝑧𝑑 = Evae(𝑑).

(2) We define the target velocity as 𝑣 = 𝑧𝐼 − 𝑧𝑑 and train the DiT 𝑓𝜃, conditioned on 𝑧𝐼 and a fixed timestep 𝑡 = 0.5, to regress it:
Llatent = ∥𝑓𝜃 (𝑧𝐼,𝑡) − 𝑣∥2 2 

. (3) Since 𝑡 is fixed, the rectified-flow parameterization reduces to a direct latent regression with no trajectory to integrate. At inference, we obtain the depth latent in a single forward pass, ˆ 
𝑧𝑑 =𝑧𝐼 − 𝑓𝜃(𝑧𝐼,𝑡), 
and decode it as 

ˆ𝑑 = Dvae(ˆ𝑧𝑑).

(4) Unlike previous diffusion-based monocular depth methods that supervise primarily in latent space
![[Pasted image 20261029082900.png]]

short 
![[Pasted image 20261029084723.png]]