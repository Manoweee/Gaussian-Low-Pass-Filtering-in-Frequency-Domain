# Gaussian-Low-Pass-Filtering-in-Frequency-Domain
## 2.1.1 Fourier Transform

The image is transformed from the spatial domain *f(x, y)* to the frequency domain *F(u, v)* using the Fourier transform:

$$
F(u,v) = \sum_{x=0}^{M-1} \sum_{y=0}^{N-1} f(x,y) \cdot e^{-j 2\pi \left( \frac{ux}{M} + \frac{vy}{N} \right)}
$$

where *f(x, y)* is the intensity value at pixel location *(x, y)*, *F(u, v)* is the complex frequency component at coordinates *(u, v)*, and *M* and *N* are the width and height of the image, respectively.
### 2.1.2 Gaussian Mask Application

A Gaussian mask is a matrix that follows a Gaussian distribution pattern, centered in the middle of the frequency domain. It assigns higher weights to the central pixels and lower weights to those near the edges, effectively reducing high-frequency noise while preserving important low-frequency information. The Gaussian mask is defined as:

$$
\text{mask}(i, j) = \exp\left(-2 \cdot r_m^2 \cdot d(i,j)^2\right)
$$

where *rₘ* is the spread (standard deviation) control parameter of the Gaussian filter in the frequency space, and *d(i, j)* is the normalized Euclidean distance of coordinate *(i, j)* from the center of the frequency domain.
### 2.1.3 High-Frequency Filtering

The Gaussian mask from Equation (2) is applied to the frequency-shifted image *F<sub>shifted</sub>(u, v)* using element-wise multiplication. This produces the denoised frequency-domain data:

$$
F_{\text{filtered}}(u, v) = F_{\text{shifted}}(u, v) \times \text{mask}(u, v)
$$

where *F<sub>filtered</sub>(u, v)* represents the filtered image in the frequency domain after applying the Gaussian mask.
### 2.1.4 Inverse Fourier Transform

The denoised image is reconstructed by applying the inverse Fourier transform to the filtered frequency-domain data:

$$
\( f_{\text{denoised}}(x, y) = \mathcal{F}^{-1} \left\{ F_{\text{filtered\_unshifted}} \right\} \)
$$

where *f<sub>denoised</sub>(x, y)* is the resulting image in the spatial domain after filtering, and *F<sub>filtered\_unshifted</sub>* is the unshifted version of the filtered frequency-domain representation *F<sub>filtered</sub>(u, v)*.


### 2.1.5 Measurement of Denoising Efficiency

The **Structural Similarity Index (SSIM)** is used to assess the similarity between the original and denoised images. It considers luminance, contrast, and structural information:

$$
\text{SSIM}(X, Y) = \frac{(\mu_X^2 + \mu_Y^2 + C_1)(\sigma_X^2 + \sigma_Y^2 + C_2)}{(2 \mu_X \mu_Y + C_1)(2 \sigma_{XY} + C_2)}
$$

where:
- *μ<sub>X</sub>* and *μ<sub>Y</sub>* are the means of images *X* and *Y*
- *σ<sub>X</sub><sup>2</sup>* and *σ<sub>Y</sub><sup>2</sup>* are the variances of *X* and *Y*
- *σ<sub>XY</sub>* is the covariance between *X* and *Y*
- *C₁* and *C₂* are small constants to stabilize the division when the denominator is close to zero
### 2.1.6 Optimization

A **Bayesian Optimization** algorithm is employed to determine the optimal Gaussian radius *rₘ* that maximizes image quality. This approach balances noise reduction and information preservation by optimizing the Structural Similarity Index (SSIM) between the original and denoised images.

