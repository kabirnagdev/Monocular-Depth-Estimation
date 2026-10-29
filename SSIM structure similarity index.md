==a perceptual metric used in computer vision and image processing to evaluate the quality of an image or video by comparing it to a clean reference image==.

depth estimation models dont have access to actual depth data in most cases  so we use a geometric trick called as **view synthesis**

MSE formula :
![[Pasted image 20261021163821.png]]

SSIM is more sophisticated :
![[Pasted image 20261021163841.png]]Why SSIM is Critical for Depth Predictions

|Feature|How L₁ / MSE Fails|How SSIM Fixes It in Depth Tasks|
|---|---|---|
|**Object Boundaries**|L₁ tolerates minor spatial misalignments if pixel colors match, creating bleed effects at edges.|**Structure (s)** strictly enforces edge correlation, ensuring crisp object boundaries and clear depth discontinuities.|
|**Lighting Changes**|If camera ego-motion changes the scene reflection or exposure, L₁ penalizes the network heavily.|**Luminance (l) & Contrast (c)** normalization allows the network to tolerate global lighting shifts while focusing purely on geometry.|
|**Textureless Regions**|In flat areas (e.g., walls, skies), L₁ gradients become uniform and fail to enforce flat depth maps.|**Local Windows (N × N)** calculate variance over local patches, assisting the network in regularizing flat planes.|