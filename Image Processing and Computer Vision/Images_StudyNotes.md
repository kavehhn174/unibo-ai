# Image Formation Process — Study Notes
### Course: Image Processing and Computer Vision | University of Bologna
### Prof. Giuseppe Lisanti | giuseppe.lisanti@unibo.it

---

## Table of Contents

1. [What is an Image?](#1-what-is-an-image)
2. [Pinhole Camera Model](#2-pinhole-camera-model)
3. [Perspective Projection](#3-perspective-projection)
4. [Properties of Perspective Projection](#4-properties-of-perspective-projection)
5. [Vanishing Points](#5-vanishing-points)
6. [Stereo Vision and 3D Reconstruction](#6-stereo-vision-and-3d-reconstruction)
7. [Standard Stereo Geometry and Disparity](#7-standard-stereo-geometry-and-disparity)
8. [Epipolar Geometry and Rectification](#8-epipolar-geometry-and-rectification)
9. [Lenses and Depth of Field](#9-lenses-and-depth-of-field)
10. [Focusing Mechanism and Diaphragm](#10-focusing-mechanism-and-diaphragm)
11. [Image Digitization](#11-image-digitization)
12. [Camera Sensors: CCD vs CMOS](#12-camera-sensors-ccd-vs-cmos)
13. [Colour Sensors and Bayer CFA](#13-colour-sensors-and-bayer-cfa)
14. [SNR and Dynamic Range](#14-snr-and-dynamic-range)
15. [📝 Review Questions (30 Questions)](#-review-questions-30-questions)

---

## 1. What is an Image?

An **image** is a 2D representation of a 3D scene. An imaging device gathers the light reflected by 3D objects and projects it onto a 2D plane.

The **image formation and acquisition process** involves three key aspects:
- **Geometric relationship**: How 3D scene points map to 2D image points.
- **Radiometric relationship**: How brightness in the image relates to the light emitted/reflected by scene objects.
- **Image digitization**: How the continuous image is converted into a discrete digital representation.

> 💡 **Additional Context (from assistant):** Think of image formation like a shadow on a wall: a 3D hand casts a 2D shadow. The "shadow" is your image. The challenge is to understand and mathematically describe this 3D → 2D mapping, and then reverse it when needed.

---

## 2. Pinhole Camera Model

The **pinhole camera** is the simplest imaging device: light passes through a tiny hole (the **pinhole**) and projects onto an **image plane** behind it.

- Geometrically, you draw straight rays from scene points through the pinhole to the image plane.
- This model is a **good approximation** of modern cameras' geometry.
- **Drawback**: A true pinhole admits very little light → extremely long exposure times → not practical for dynamic scenes.

![Pinhole camera model diagram](images/slide_03_figure_01_pinhole_model.png)
> 📌 **Note:** This image is located on **Slide 3** of the provided slides (figure title: "Pinhole camera model"). Please place the file as `images/slide_03_figure_01_pinhole_model.png` in the same GitHub repository — it will load automatically once added.

> ⚠️ **Exam Tip:** Know the conceptual definition of the pinhole camera. It is the foundation of all subsequent camera models.

---

## 3. Perspective Projection

The geometric model of image formation in a pinhole camera is called **Perspective Projection**.

### Key Terminology

| Symbol | Meaning |
|--------|---------|
| $M$ | Scene point (3D) |
| $m$ | Corresponding image point (2D) |
| $I$ | Image plane |
| $C$ | Optical centre (the pinhole) |
| Optical axis | Line through $C$, orthogonal to $I$ |
| $c$ | Image centre (piercing point) — intersection of optical axis and $I$ |
| $f$ | Focal length |
| $F$ | Focal plane |

### The Projection Equations

Using the **camera reference system** (axes parallel to image axes), perspective projection maps a 3D point $(x, y, z)$ to image coordinates $(u, v)$:

$$u = x \frac{f}{z}, \quad v = y \frac{f}{z}$$

> ⚠️ **Exam Tip:** These two equations are fundamental. You **must** know them. The "minus sign" version applies when the image plane is behind the optical centre (physically flipped image). In practice, we place the image plane in **front** of the optical centre to avoid inversion, yielding the positive-sign version above.

**Key observations:**
- Image coordinates are a **scaled version** of scene coordinates, with the scale factor being $f/z$.
- As $z$ increases (farther object), $u$ and $v$ get **smaller** → distant objects look smaller.
- As $f$ increases (longer focal length), $u$ and $v$ get **bigger** → zoom effect.
- The mapping is **not a bijection**: one 3D point → one 2D point (injective), but one 2D image point → an entire 3D **line** (not invertible).

> 💡 **Additional Context (from assistant):** The scale factor $f/z$ is why a telephoto lens (large $f$) magnifies objects and a wide-angle lens (small $f$) gives a broader view. Also, the non-invertibility is why depth perception from a single photo is impossible without additional info (e.g., known object sizes or stereo).

### Why the Mapping is NOT a Bijection

- A **scene point** maps to exactly **one** image point ✅
- An **image point** maps back to an **infinite line** in 3D (the ray through $m$ and $C$) ❌ for unique inversion
- Therefore, **recovering 3D structure from a single image is ill-posed** (infinite solutions).

> ⚠️ **Exam Tip:** "Ill-posed problem" is a key term. Explain why single-image 3D recovery is ill-posed.

---

## 4. Properties of Perspective Projection

### Length Scaling

A 3D segment of length $L$ lying in a plane **parallel** to the image plane at depth $z$ appears in the image with length:

$$l = \frac{f \cdot L}{z}$$

> ⚠️ **Exam Tip:** Know this formula. It explains why objects appear smaller as they move away.

**Example:** If $L = 2\,\text{m}$, $f = 50\,\text{mm} = 0.05\,\text{m}$, and $z = 10\,\text{m}$:
$$l = \frac{0.05 \times 2}{10} = 0.01\,\text{m} = 10\,\text{mm}$$

**Additional examples generated:**

*Example 1:* $L = 3\,\text{m}$, $f = 35\,\text{mm}$, $z = 5\,\text{m}$  
$$l = \frac{0.035 \times 3}{5} = 0.021\,\text{m} = 21\,\text{mm}$$

*Example 2:* $L = 1.8\,\text{m}$ (person), $f = 85\,\text{mm}$, $z = 20\,\text{m}$  
$$l = \frac{0.085 \times 1.8}{20} = 0.00765\,\text{m} = 7.65\,\text{mm}$$

*Example 3:* $L = 0.5\,\text{m}$, $f = 24\,\text{mm}$, $z = 2\,\text{m}$  
$$l = \frac{0.024 \times 0.5}{2} = 0.006\,\text{m} = 6\,\text{mm}$$

### Other Properties

- **3D lines → 2D lines**: Perspective projection preserves straight lines (a 3D line becomes a 2D line in the image).
- **Length ratios NOT preserved**: Unless the scene is planar and parallel to the image plane.
- **Parallelism NOT preserved**: Parallel 3D lines generally do **not** appear parallel in the image (exception: lines parallel to the image plane).

---

## 5. Vanishing Points

**Definition:** The images of parallel 3D lines intersect at a single point called the **vanishing point**.

- If the lines are parallel to the image plane, they meet at infinity (no finite vanishing point).
- Vanishing points may lie **outside** the image frame.

![Vanishing point diagram](images/slide_23_figure_01_vanishing_point.png)
> 📌 **Note:** This image is located on **Slide 23** of the provided slides. Please place the file as `images/slide_23_figure_01_vanishing_point.png` in the same GitHub repository.

> 💡 **Additional Context (from assistant):** Classic example — railway tracks. They are parallel in the real world but appear to converge to a vanishing point on the horizon in photos. This is a direct consequence of perspective projection.

> ⚠️ **Exam Tip:** Be able to explain why vanishing points exist and identify them in a given image scenario.

---

## 6. Stereo Vision and 3D Reconstruction

### The Problem

From a single image, we cannot recover depth (the 3D position of a scene point is ambiguous — it lies somewhere along a ray).

### The Solution: Stereo Vision

Use **at least two images** from different viewpoints → enables depth inference by **triangulation**.

- The **human visual system** is naturally a stereo system (two eyes).
- Given **corresponding points** in two images, the 3D point can be recovered geometrically.

> ⚠️ **Exam Tip:** Triangulation from stereo is a fundamental concept. Know that two rays from two cameras intersect at the 3D scene point.

---

## 7. Standard Stereo Geometry and Disparity

### Assumptions of Standard Stereo Geometry

1. Both cameras have **parallel axes** ($x$, $y$, $z$).
2. Same **focal length** $f$ → coplanar image planes.
3. The transformation between the two camera frames is a **pure horizontal translation** by baseline $b$.
4. Both cameras image the scene **simultaneously**.

![Standard stereo geometry diagram](images/slide_14_figure_01_stereo_geometry.png)
> 📌 **Note:** This image is located on **Slide 14** of the provided slides. Please place the file as `images/slide_14_figure_01_stereo_geometry.png` in the same GitHub repository.

### Projection Equations for Both Cameras

$$u_L = \frac{x_L \cdot f}{z}, \quad u_R = \frac{x_R \cdot f}{z}$$
$$v_L = v_R = \frac{y \cdot f}{z}$$

Since $x_L - x_R = b$ (baseline), we get:

$$u_L - u_R = \frac{b \cdot f}{z}$$

### Disparity

**Disparity** $d$ is the horizontal difference between corresponding image coordinates:

$$d = u_L - u_R = \frac{b \cdot f}{z}$$

Therefore, depth $z$ can be recovered as:

$$\boxed{z = \frac{b \cdot f}{d}}$$

> ⚠️ **Exam Tip:** This is the **fundamental relationship in stereo vision**. You must know it and understand it. Disparity and depth are inversely proportional.

**Key insight:**
- **Large disparity** → point is **close** to the camera (small $z$).
- **Small disparity** → point is **far** from the camera (large $z$).

### Worked Example

Given: $b = 0.12\,\text{m}$, $f = 600\,\text{px}$, $d = 30\,\text{px}$. Find $z$.

$$z = \frac{b \cdot f}{d} = \frac{0.12 \times 600}{30} = 2.4\,\text{m}$$

### 3 Additional Stereo Disparity Examples

**Example 1:** $b = 0.10\,\text{m}$, $f = 500\,\text{px}$, $d = 25\,\text{px}$
$$z = \frac{0.10 \times 500}{25} = 2.0\,\text{m}$$

**Example 2:** $b = 0.20\,\text{m}$, $f = 800\,\text{px}$, $d = 16\,\text{px}$
$$z = \frac{0.20 \times 800}{16} = 10.0\,\text{m}$$

**Example 3:** $b = 0.06\,\text{m}$, $f = 400\,\text{px}$, $d = 48\,\text{px}$
$$z = \frac{0.06 \times 400}{48} = 0.5\,\text{m}$$

### Stereo Matching

Given a point $p_L$ in the left image, finding the corresponding $p_R$ in the right image is called **stereo matching** (or stereo correspondence).

- In standard stereo geometry, corresponding points lie on the **same horizontal line** ($v_L = v_R$), so we only search along that line (1D search).
- A common technique: **block matching** — compare pixel windows around candidate points.

---

## 8. Epipolar Geometry and Rectification

### When Cameras Are Not Aligned

If the two cameras are **not perfectly horizontally aligned**, corresponding points no longer lie on the same horizontal scanline. Instead, corresponding point $p_R$ must be found along an **epipolar line** in the right image.

**Key fact:** The search space for stereo correspondence is always **1D** (a line), regardless of camera orientation.

![Epipolar geometry diagram](images/slide_17_figure_01_epipolar_geometry.png)
> 📌 **Note:** This image is located on **Slide 17** of the provided slides. Please place the file as `images/slide_17_figure_01_epipolar_geometry.png` in the same GitHub repository.

### Problem with Non-Standard Geometry

- Epipolar lines are **oblique** → searching along them is awkward and computationally inefficient.

### Solution: Rectification

**Rectification** (also called warping) transforms both images so that epipolar lines become **horizontal and collinear** — effectively converting the geometry to a standard stereo setup.

- A **homography** (projective transformation) is applied to both images.
- After rectification, standard horizontal-line stereo matching can be applied.

> ⚠️ **Exam Tip:** Know what rectification is and why it's needed. Understand that it converts oblique epipolar lines into horizontal lines to simplify matching.

> 💡 **Additional Context (from assistant):** In practice, virtually all modern stereo camera systems perform rectification as a preprocessing step. The OpenCV library, for instance, provides `cv2.stereoRectify()` for exactly this purpose.

---

## 9. Lenses and Depth of Field

### Why Lenses?

The pinhole camera requires a tiny aperture → very little light → long exposure times → motion blur for dynamic scenes.

**Lenses** gather more light from a scene point and **focus** it onto a single image point, enabling much shorter exposure times.

### The Thin Lens Equation

$$\frac{1}{d_S} + \frac{1}{d_I} = \frac{1}{f_L}$$

Where:

| Symbol | Meaning |
|--------|---------|
| $d_S$ | Distance from scene point $P$ to the lens |
| $d_I$ | Distance from focused image point $p$ to the lens |
| $f_L$ | Focal length of the lens (physical parameter of the lens) |

### Rearranged Forms

To find the required image plane distance given scene distance:
$$d_I = \frac{d_S \cdot f_L}{d_S - f_L}$$

To find scene distance given image plane position:
$$d_S = \frac{d_I \cdot f_L}{d_I - f_L}$$

> ⚠️ **Exam Tip:** The thin lens equation is very likely to appear in an exam. Know all three forms and be able to use them.

### Worked Example

A scene point is at $d_S = 3\,\text{m}$ from a lens with $f_L = 50\,\text{mm} = 0.05\,\text{m}$. Where must the image plane be?

$$d_I = \frac{3 \times 0.05}{3 - 0.05} = \frac{0.15}{2.95} \approx 0.0508\,\text{m} \approx 50.8\,\text{mm}$$

### 3 Additional Thin Lens Examples

**Example 1:** $d_S = 1\,\text{m}$, $f_L = 50\,\text{mm} = 0.05\,\text{m}$
$$d_I = \frac{1 \times 0.05}{1 - 0.05} = \frac{0.05}{0.95} \approx 52.6\,\text{mm}$$

**Example 2:** $d_S = 5\,\text{m}$, $f_L = 35\,\text{mm} = 0.035\,\text{m}$
$$d_I = \frac{5 \times 0.035}{5 - 0.035} = \frac{0.175}{4.965} \approx 35.3\,\text{mm}$$

**Example 3:** $d_S = 0.3\,\text{m}$, $f_L = 100\,\text{mm} = 0.1\,\text{m}$
$$d_I = \frac{0.3 \times 0.1}{0.3 - 0.1} = \frac{0.03}{0.2} = 0.15\,\text{m} = 150\,\text{mm}$$

### Circles of Confusion and Depth of Field

- When a scene point is NOT at the focusing distance, its light rays do not converge to a single point on the image plane → they form a **Circle of Confusion** (blur circle).
- The **Depth of Field (DOF)** is the range of scene distances over which blur circles are small enough to appear in focus (i.e., smaller than one pixel/photosensing element).

![Circles of confusion / DOF diagram](images/slide_29_figure_01_circles_of_confusion.png)
> 📌 **Note:** This image is located on **Slide 29** of the provided slides. Please place the file as `images/slide_29_figure_01_circles_of_confusion.png` in the same GitHub repository.

---

## 10. Focusing Mechanism and Diaphragm

### Focusing Mechanism

To focus on objects at different distances, the lens (or lens subsystem) **translates along the optical axis** relative to the fixed image plane position.

- When $d_I = f_L$: camera is focused at **infinity** (scene point infinitely far).
- As $d_I$ increases (lens moves farther from image plane): $d_S$ decreases → camera focuses on **closer** objects.

$$\text{Closer focus} \iff d_I \uparrow \iff d_S \downarrow$$

> ⚠️ **Exam Tip:** Understand the inverse relationship between $d_I$ and $d_S$ from the thin lens equation.

### Diaphragm (Iris)

The **diaphragm** (or iris) is an adjustable aperture controlling how much light enters the camera.

| Aperture | Light | Blur Circle Size | DOF | Exposure Time |
|----------|-------|-----------------|-----|---------------|
| Small (closed) | Less | Smaller | Larger (more in focus) | Must increase |
| Large (open) | More | Larger | Smaller (less in focus) | Can decrease |

**The tradeoff:**
- Close diaphragm → large DOF ✅, but need longer exposure → risk of motion blur ❌
- Open diaphragm → short exposure ✅, but small DOF → only a narrow depth band is in focus ❌

> ⚠️ **Exam Tip:** Understand and be able to articulate this DOF vs. motion blur tradeoff.

> 💡 **Additional Context (from assistant):** In photography, the aperture is described by the **f-number** (f/2.8, f/8, etc.). A small f-number = large aperture = shallow DOF (blurry background, used for portraits). A large f-number = small aperture = large DOF (everything sharp, used for landscapes).

---

## 11. Image Digitization

A real camera sensor converts continuous light intensity into a **digital image** through two steps:

### 1. Sampling

The continuous 2D image is sampled on a **regular grid** of $N \times M$ points. Each grid point is called a **pixel** (picture element).

### 2. Quantization

The continuous intensity at each pixel is mapped to one of $l = 2^m$ discrete **gray levels** where $m$ is the number of bits per pixel.

**Memory occupancy (gray-scale):**
$$B = N \times M \times m \quad \text{(bits)}$$

**Standard values:**
- $m = 8$ bits/pixel → 256 gray levels (gray-scale)
- $m = 24$ bits/pixel (3 × 8) for colour (RGB)
- VGA ($480 \times 640$, $m=8$): $\approx 300\,\text{KB}$
- 1 Megapixel ($1000 \times 1000$, $m=8$): $\approx 1\,\text{MB}$

> ⚠️ **Exam Tip:** Know the memory formula $B = N \times M \times m$. Be able to calculate storage for given image dimensions and bit depth.

### Worked Example

Calculate storage for a $1920 \times 1080$ RGB image:
$$B = 1920 \times 1080 \times 24 = 49{,}766{,}400\,\text{bits} = 6{,}220{,}800\,\text{bytes} \approx 5.93\,\text{MB}$$

### 3 Additional Storage Examples

**Example 1:** $640 \times 480$, $m=8$ (gray-scale):
$$B = 640 \times 480 \times 8 = 2{,}457{,}600\,\text{bits} = 307{,}200\,\text{bytes} \approx 300\,\text{KB}$$

**Example 2:** $3000 \times 2000$, $m=8$ (gray-scale):
$$B = 3000 \times 2000 \times 8 = 48{,}000{,}000\,\text{bits} = 6{,}000{,}000\,\text{bytes} = 6\,\text{MB}$$

**Example 3:** $1280 \times 720$ (HD), $m=24$ (RGB):
$$B = 1280 \times 720 \times 24 = 22{,}118{,}400\,\text{bits} = 2{,}764{,}800\,\text{bytes} \approx 2.64\,\text{MB}$$

### Effect of Sampling and Quantization on Quality

- **Coarser sampling** (fewer pixels): loss of fine details, jagged contours (**aliasing**).
- **Coarser quantization** (fewer gray levels): false contours, banding artifacts.

![Digitization quality comparison](images/slide_36_figure_01_digitization_quality.png)
> 📌 **Note:** This image is located on **Slide 36** of the provided slides. Please place the file as `images/slide_36_figure_01_digitization_quality.png` in the same GitHub repository.

> 💡 **Additional Context (from assistant):** **Aliasing** occurs when the sampling rate is too low to faithfully capture the spatial frequencies in the scene — related to the Nyquist-Shannon sampling theorem. In images, this manifests as jagged diagonal lines ("staircasing") on coarsely sampled images.

---

## 12. Camera Sensors: CCD vs CMOS

Modern digital cameras use solid-state 2D arrays of photodetectors. During **exposure time**, each detector converts incident photons into proportional electric charge.

### CCD vs CMOS Comparison

| Feature | CCD | CMOS |
|---------|-----|------|
| Full name | Charge-Coupled Device | Complementary Metal Oxide Semiconductor |
| Integration | Sensor and circuitry separate | All on one chip ("one-chip camera") |
| Power consumption | Higher | Lower |
| Cost | Higher | Lower |
| Compactness | Less compact | More compact |
| ROI readout | Must read full frame | Can read arbitrary window (ROI) |
| SNR | Higher | Lower (typically) |
| Dynamic Range | Higher | Lower (typically) |
| Uniformity | Better | Slightly worse |

> ⚠️ **Exam Tip:** Know the key differences between CCD and CMOS, especially the advantages of CMOS (integration, ROI readout, cost) and of CCD (SNR, DR, uniformity).

> 💡 **Additional Context (from assistant):** Today, CMOS sensors dominate in smartphones, webcams, and most consumer cameras due to cost and integration advantages. High-end scientific and astronomical cameras still use CCD for superior SNR and DR.

---

## 13. Colour Sensors and Bayer CFA

### Why Standard Sensors Cannot Sense Colour

CCD/CMOS sensors respond to light from **near-UV (~200 nm)** through **near-infrared (~1100 nm)**, integrating over all wavelengths. They cannot distinguish wavelengths → they are inherently **monochromatic**.

### Colour Filter Array (CFA)

An array of optical filters placed **in front of the photodetectors** makes each pixel sensitive to a specific colour range.

**Bayer CFA** (most common):
- Pattern: RGGB (Red, Green, Green, Blue).
- Green filters are **twice as numerous** as red or blue → mimics the human eye's higher sensitivity to green.
- Missing colour values at each pixel are estimated from neighbours → **demosaicking**.
- True sensor resolution is **lower** than nominal: green subsampled by 2×, red/blue by 4×.

![Bayer CFA pattern diagram](images/slide_39_figure_01_bayer_cfa.png)
> 📌 **Note:** This image is located on **Slide 39** of the provided slides. Please place the file as `images/slide_39_figure_01_bayer_cfa.png` in the same GitHub repository.

### Full-Resolution Colour Alternative

Use an **optical prism** to split incoming light into 3 RGB beams → send each to a separate sensor with the corresponding filter → full resolution for all channels, but more expensive (3-chip camera).

> ⚠️ **Exam Tip:** Know what the Bayer CFA is, why green is doubled, and what demosaicking means.

---

## 14. SNR and Dynamic Range

### Signal-to-Noise Ratio (SNR)

Under static conditions, pixel values are **not deterministic** due to random noise. The SNR measures the strength of the true signal relative to noise fluctuations.

**Main noise sources:**
1. **Photon Shot Noise** – Photon arrival times follow a Poisson distribution → variable photon count during exposure.
2. **Electronic Circuitry Noise** – From read-out and amplification electronics.
3. **Quantization Noise** – Due to the ADC conversion.
4. **Dark Current Noise** – Thermal excitation generates charge even with no light.

**SNR in decibels and bits:**
$$\text{SNR}_{dB} = 20 \log_{10}(\text{SNR}), \quad \text{SNR}_{bit} = \log_2(\text{SNR})$$

> ⚠️ **Exam Tip:** Know the 4 noise sources and the SNR formula in dB and bits.

### Dynamic Range (DR)

**DR** measures the ratio between the **maximum** detectable irradiance (saturation) and the **minimum** (threshold above noise floor):

$$\text{DR} = \frac{E_{max}}{E_{min}}$$

Also expressed in dB or bits (same formulas as SNR).

**Higher DR → better ability to simultaneously capture both dark and bright parts of a scene.**

> 💡 **Additional Context (from assistant):** **HDR (High Dynamic Range)** imaging addresses cameras' limited DR by combining multiple exposures (e.g., one for shadows, one for highlights) into a single image. Smartphones now do this automatically with "Night Mode" or "HDR+" features.

> ⚠️ **Exam Tip:** Understand what DR means physically and know the $E_{max}/E_{min}$ definition.

---

## 📝 Review Questions (30 Questions)

---

### Q1. What is an image in the context of image formation? ⚠️

**Answer:**  
An image is a **2D projection** of a 3D scene. An imaging device collects light reflected by 3D objects and maps them onto a 2D image plane. The image formation process involves three aspects: (1) geometric relationships (3D→2D mapping), (2) radiometric relationships (brightness mapping), and (3) digitization (converting to discrete digital format).

---

### Q2. What is the pinhole camera model and what are its practical limitations?

**Answer:**  
The **pinhole camera** uses a tiny hole through which light passes and projects onto an image plane. Geometrically, every scene point connects to the image via a straight ray through the pinhole.  
**Limitations:**  
- Very small aperture → minimal light gathered → requires extremely long exposure times.
- Long exposures → motion blur for any moving subject.
- Not practical for real-world dynamic scene capture.

---

### Q3. Write the perspective projection equations. What does each variable represent? ⚠️

**Answer:**  
$$u = \frac{f \cdot x}{z}, \quad v = \frac{f \cdot y}{z}$$  
- $(u, v)$: 2D image coordinates
- $(x, y, z)$: 3D scene point coordinates in the camera reference frame
- $f$: focal length

---

### Q4. What happens to the image coordinates $u$ and $v$ as $z$ increases? Explain physically. ⚠️

**Answer:**  
As $z$ increases (scene point moves farther), the scale factor $f/z$ **decreases**, so $u$ and $v$ get smaller. This means the object appears **smaller** in the image — which matches everyday experience (farther objects look smaller). This is the essence of perspective projection.

---

### Q5. A scene point is at $(x, y, z) = (3, 2, 10)$ and $f = 20$. Find the image coordinates $(u, v)$.

**Answer:**  
$$u = \frac{20 \times 3}{10} = 6, \quad v = \frac{20 \times 2}{10} = 4$$  
Image coordinates: $(u, v) = (6, 4)$.

**3 similar examples:**

*Ex 1:* $(x, y, z) = (5, -3, 15)$, $f = 30$:  
$$u = \frac{30 \times 5}{15} = 10, \quad v = \frac{30 \times (-3)}{15} = -6$$

*Ex 2:* $(x, y, z) = (1, 1, 5)$, $f = 10$:  
$$u = \frac{10 \times 1}{5} = 2, \quad v = \frac{10 \times 1}{5} = 2$$

*Ex 3:* $(x, y, z) = (4, 0, 8)$, $f = 16$:  
$$u = \frac{16 \times 4}{8} = 8, \quad v = 0$$

---

### Q6. Why is single-image 3D reconstruction an ill-posed problem? ⚠️

**Answer:**  
The perspective projection maps a **3D line** (all points on the ray from $C$ through $m$) to a **single 2D image point**. Therefore, given only the 2D image point, we cannot determine where on the 3D ray the actual scene point lies. The solution is not unique — infinitely many 3D points correspond to one image point. This makes single-image 3D reconstruction **ill-posed**.

---

### Q7. A 3D segment of length $L = 4\,\text{m}$ lies parallel to the image plane at depth $z = 8\,\text{m}$. If $f = 40\,\text{mm}$, find its image length. ⚠️

**Answer:**  
$$l = \frac{f \cdot L}{z} = \frac{0.04 \times 4}{8} = 0.02\,\text{m} = 20\,\text{mm}$$

---

### Q8. Is parallelism preserved under perspective projection? Give an example.

**Answer:**  
**No**, parallelism is generally **not** preserved. Parallel 3D lines (not parallel to the image plane) appear to converge in the image at a **vanishing point**.  
**Example:** Railway tracks are parallel in the real world but appear to converge in a photograph.  
**Exception:** Lines parallel to the image plane remain parallel in the image.

---

### Q9. What is a vanishing point? ⚠️

**Answer:**  
A **vanishing point** is the image point where the projections of two or more parallel 3D lines (not parallel to the image plane) converge. It may lie outside the image frame. If the parallel lines are parallel to the image plane, the vanishing point is at infinity.

---

### Q10. How does stereo vision solve the single-image depth ambiguity? ⚠️

**Answer:**  
By using **two cameras** (at known positions), each image point generates a **ray** in 3D. The two rays (one from each camera) corresponding to the same scene point intersect at the actual **3D location** of that point — a process called **triangulation**. This resolves the depth ambiguity that exists in a single image.

---

### Q11. What is the baseline in a stereo camera setup?

**Answer:**  
The **baseline** $b$ is the horizontal distance between the optical centres of the two cameras ($O_L$ and $O_R$). It is the translational displacement between the two cameras in the standard stereo geometry.

---

### Q12. Define disparity and write the fundamental stereo formula. ⚠️

**Answer:**  
**Disparity** $d$ is the horizontal difference between corresponding image coordinates in the left and right images:
$$d = u_L - u_R$$  
The fundamental stereo relationship is:
$$z = \frac{b \cdot f}{d}$$
where $z$ is depth, $b$ is baseline, and $f$ is focal length.

---

### Q13. A stereo system has $b = 0.15\,\text{m}$, $f = 700\,\text{px}$. A point has disparity $d = 35\,\text{px}$. Find depth $z$. ⚠️

**Answer:**  
$$z = \frac{b \cdot f}{d} = \frac{0.15 \times 700}{35} = \frac{105}{35} = 3.0\,\text{m}$$

**3 similar examples:**

*Ex 1:* $b = 0.08\,\text{m}$, $f = 400\,\text{px}$, $d = 20\,\text{px}$:  
$$z = \frac{0.08 \times 400}{20} = 1.6\,\text{m}$$

*Ex 2:* $b = 0.25\,\text{m}$, $f = 1000\,\text{px}$, $d = 50\,\text{px}$:  
$$z = \frac{0.25 \times 1000}{50} = 5.0\,\text{m}$$

*Ex 3:* $b = 0.05\,\text{m}$, $f = 300\,\text{px}$, $d = 60\,\text{px}$:  
$$z = \frac{0.05 \times 300}{60} = 0.25\,\text{m}$$

---

### Q14. What is the relationship between disparity and depth? Is it linear? ⚠️

**Answer:**  
Disparity and depth are **inversely proportional**: $d = bf/z$. As $z$ increases, $d$ decreases and vice versa. The relationship is **not linear** — it is hyperbolic. Objects close to the camera have large disparity; distant objects have small disparity.

---

### Q15. What are the assumptions required for standard stereo geometry?

**Answer:**  
1. Cameras have **parallel** $(x, y, z)$ axes.
2. Same **focal length** $f$ → coplanar image planes.
3. The transformation between frames is a **pure horizontal translation** by baseline $b$.
4. Both cameras capture images **simultaneously**.

---

### Q16. What is stereo matching (stereo correspondence)? How is it done in standard geometry?

**Answer:**  
**Stereo matching** (or **stereo correspondence**) is the process of finding, for a given point $p_L$ in the left image, the corresponding point $p_R$ in the right image that represents the same 3D scene point.  
In **standard stereo geometry**, $p_R$ must lie on the **same horizontal scanline** as $p_L$ (since $v_L = v_R$), reducing search to 1D. A common method is **block matching**: comparing a window of pixels around candidate points for similarity (e.g., by SSD or NCC).

---

### Q17. What is epipolar geometry? What is an epipolar line? ⚠️

**Answer:**  
**Epipolar geometry** describes the geometric relationship between two non-standard cameras. For a point $p_L$ in the left image, its corresponding point $p_R$ in the right image must lie on a specific line in the right image called the **epipolar line** — the projection of the 3D ray associated with $p_L$ into the right camera plane. This reduces the stereo correspondence search from 2D (entire image) to 1D (a line).

---

### Q18. What is image rectification and why is it needed? ⚠️

**Answer:**  
**Rectification** warps both stereo images using a **homography** (projective transformation) so that all epipolar lines become **horizontal and collinear** — equivalent to standard stereo geometry.  
It is needed because real stereo rigs are almost never perfectly horizontally aligned, making epipolar lines oblique. Rectification makes stereo matching faster and more straightforward (horizontal 1D search).

---

### Q19. Write and explain the thin lens equation. ⚠️

**Answer:**  
$$\frac{1}{d_S} + \frac{1}{d_I} = \frac{1}{f_L}$$  
- $d_S$: distance from scene point to lens  
- $d_I$: distance from focused image point to lens  
- $f_L$: focal length of the lens (intrinsic property)  

This equation relates where an object must be (scene side) to where its focused image appears (image side) for a given lens. If $d_S \to \infty$, then $d_I \to f_L$ (focus at infinity).

---

### Q20. A lens has $f_L = 35\,\text{mm}$. A scene point is at $d_S = 2\,\text{m}$. Where is the focused image? ⚠️

**Answer:**  
$$d_I = \frac{d_S \cdot f_L}{d_S - f_L} = \frac{2 \times 0.035}{2 - 0.035} = \frac{0.07}{1.965} \approx 35.6\,\text{mm}$$

**3 similar examples:**

*Ex 1:* $f_L = 50\,\text{mm}$, $d_S = 1.5\,\text{m}$:  
$$d_I = \frac{1.5 \times 0.05}{1.5 - 0.05} = \frac{0.075}{1.45} \approx 51.7\,\text{mm}$$

*Ex 2:* $f_L = 24\,\text{mm}$, $d_S = 0.5\,\text{m}$:  
$$d_I = \frac{0.5 \times 0.024}{0.5 - 0.024} = \frac{0.012}{0.476} \approx 25.2\,\text{mm}$$

*Ex 3:* $f_L = 100\,\text{mm}$, $d_S = 5\,\text{m}$:  
$$d_I = \frac{5 \times 0.1}{5 - 0.1} = \frac{0.5}{4.9} \approx 102.0\,\text{mm}$$

---

### Q21. What is a Circle of Confusion? When does it appear?

**Answer:**  
A **Circle of Confusion** (or blur circle) forms when a scene point is NOT at the focusing distance. Its light rays through the lens do not converge to a point on the image plane, but instead form a small circle. If the circle is smaller than one pixel, the image still appears sharp. If larger, the image appears blurred (out of focus).

---

### Q22. What is Depth of Field (DOF)? ⚠️

**Answer:**  
**Depth of Field (DOF)** is the range of scene distances over which objects appear acceptably sharp in the image. Specifically, it is the depth range for which circles of confusion remain smaller than the photosensing element size (pixel size). 

Factors affecting DOF:
- **Larger aperture** → smaller DOF (only a narrow depth band is sharp)
- **Smaller aperture** → larger DOF (wider depth range is sharp)
- **Longer focal length** → smaller DOF

---

### Q23. Explain the DOF vs. motion blur tradeoff. ⚠️

**Answer:**  
- To get **large DOF**: reduce aperture (close the diaphragm) → less light → must increase **exposure time** → risk of **motion blur** for moving subjects.
- To avoid **motion blur**: use short exposure → need more light → open aperture → **shallow DOF**.  
These goals are in direct conflict. In practice, photographers balance them using ISO sensitivity, lighting conditions, and lens choice.

---

### Q24. Describe the focusing mechanism of a real lens.

**Answer:**  
The lens (or lens group) **translates along the optical axis** relative to the fixed image plane.
- When $d_I = f_L$: camera is focused at infinity.
- As $d_I$ increases (lens moves farther from sensor): camera focuses on closer objects (smaller $d_S$).
- The maximum $d_I$ corresponds to the camera's **minimum focusing distance**.

---

### Q25. What is image sampling? What is quantization? ⚠️

**Answer:**  
- **Sampling:** Converting the continuous 2D image into a discrete $N \times M$ grid of pixels. Each pixel represents the image value at one grid point.
- **Quantization:** Mapping the continuous intensity at each pixel to one of $l = 2^m$ discrete gray levels (integers), where $m$ is the number of bits per pixel.

---

### Q26. Calculate memory for a $4000 \times 3000$ RGB image with 8 bits per channel. ⚠️

**Answer:**  
Total bits per pixel = $3 \times 8 = 24$ bits.
$$B = 4000 \times 3000 \times 24 = 288{,}000{,}000\,\text{bits} = 36{,}000{,}000\,\text{bytes} = 36\,\text{MB}$$

**3 similar examples:**

*Ex 1:* $800 \times 600$, gray ($m=8$):  
$$B = 800 \times 600 \times 8 = 3{,}840{,}000\,\text{bits} = 480{,}000\,\text{B} \approx 469\,\text{KB}$$

*Ex 2:* $1920 \times 1080$, RGB ($m=24$):  
$$B = 1920 \times 1080 \times 24 = 49{,}766{,}400\,\text{bits} \approx 5.93\,\text{MB}$$

*Ex 3:* $256 \times 256$, $m=4$ bits:  
$$B = 256 \times 256 \times 4 = 262{,}144\,\text{bits} = 32{,}768\,\text{B} = 32\,\text{KB}$$

---

### Q27. What is aliasing in digital images?

**Answer:**  
**Aliasing** occurs when the sampling rate (pixel density) is too low to represent the spatial frequencies in the image. This violates the Nyquist-Shannon theorem. The result is:
- Jagged, staircase-like edges ("staircasing") on diagonal lines.
- Loss of fine detail.
- False patterns (moiré effects) when high-frequency content is undersampled.

---

### Q28. What are the key differences between CCD and CMOS sensors? ⚠️

**Answer:**  

| | CCD | CMOS |
|--|-----|------|
| Integration | Sensor separate from circuitry | All on one chip |
| Power | Higher | Lower |
| Cost | Higher | Lower |
| SNR/DR | Higher | Lower (typically) |
| ROI readout | Full frame required | Arbitrary window possible |
| Uniformity | Better | Slightly worse |

CMOS dominates consumer devices; CCD is preferred for scientific/high-quality imaging.

---

### Q29. What is the Bayer CFA and why is green doubled? ⚠️

**Answer:**  
The **Bayer Colour Filter Array (CFA)** is an RGGB pattern of colour filters placed in front of photodetectors to enable colour sensing. Each pixel is sensitive to only one colour channel.  
**Green is doubled** (2G : 1R : 1B) because the human visual system is most sensitive to green wavelengths — this design mimics human perception and maximises perceived image quality.  
Missing colour values are estimated from neighbouring pixels through **demosaicking**.

---

### Q30. Define SNR and Dynamic Range. List the four main noise sources. ⚠️

**Answer:**  
**SNR (Signal-to-Noise Ratio):** Quantifies the strength of the true signal relative to noise:
$$\text{SNR}_{dB} = 20\log_{10}(\text{SNR}), \quad \text{SNR}_{bit} = \log_2(\text{SNR})$$

**Dynamic Range (DR):** The ratio between maximum (saturation) and minimum (detectable) irradiance:
$$\text{DR} = \frac{E_{max}}{E_{min}}$$

**Four main noise sources:**
1. **Photon Shot Noise** (Poisson statistics of photon arrival)
2. **Electronic Circuitry Noise** (read-out amplification)
3. **Quantization Noise** (ADC discretization)
4. **Dark Current Noise** (thermal excitation even in darkness)
