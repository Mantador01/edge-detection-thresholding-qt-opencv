# 🧭 Edge Detection & Thresholding — C++ (Qt + OpenCV)

Comprehensive lab project for **Image Analysis** at **Université Claude Bernard Lyon 1** focused on **edge detection** and **thresholding**.  
Implements **Prewitt, Sobel, Kirsch** in multiple directions (0°, 45°, 90°, 135°), **bidirectional vs. multidirectional** gradient analysis, **global vs. hysteresis thresholding**, **non‑maximum suppression (NMS)** for contour thinning, and **colorized gradient visualization**. Provides a **Qt GUI** and organized outputs.

Also check the ```/Rapport/rapport.md```

---

## 🎯 Objectives

- Compute **gradient vectors** and visualize **magnitude** and **direction**.
- Compare **bidirectional (X/Y)** vs **multidirectional (0°,45°,90°,135°)** detection.
- Apply **global thresholding** and **double-threshold hysteresis**.
- **Thin edges** using **non‑maximum suppression**.
- Provide an **interactive Qt UI** with tunable parameters (filter type, thresholds, etc.).

---

## 🧩 Repository Structure

```
code/
├─ CMakeLists.txt
├─ image/                         # input images
├─ resultat/                      # generated results (jpg/png)
├─ Rapport/                       # report & figures
├─ main.cpp                       # Qt app entry
├─ mywindow.h / mywindow.cpp      # GUI logic + processing
├─ multi_directional_filters.cpp  # 4-direction filters
├─ gradient_detection.cpp         # bidirectional & multidirectional gradient
├─ gradient_threshold.cpp         # global & hysteresis thresholding
```

> A **build/** folder (CMake cache, autogen) may be present locally but is not required in the repo.

---

## 🧪 Features

### 1) Gradient Filters
- **Prewitt** — uniform weights, simple derivative approximation.  
- **Sobel** — stronger center weights (±2), enhances local transitions.  
- **Kirsch** — directional emphasis via higher absolute weights.

**Directions:** 0°, 90°, 45°, 135°.  
**Combination:** by absolute‑sum, or by **max‑response selection** (keeps the dominant direction per pixel).

### 2) Bidirectional vs. Multidirectional
- **Bidirectional (X/Y)**: compute \(G_x, G_y\), magnitude \(\sqrt{G_x^2 + G_y^2}\), direction \(\theta = \mathrm{atan2}(G_y, G_x)\).  
- **Multidirectional (0°,45°,90°,135°)**: take the **max response** among directions to preserve dominant orientation and reduce mixing artifacts.

### 3) Thresholding
- **Global threshold (single T)**: T chosen from **mean gradient magnitude**.  
- **Hysteresis (double thresholds)**: keep **strong** edges (\(\ge T_h\)) and keep **weak** edges (\(\ge T_l\)) **only if connected** to strong ones.

### 4) Non‑Maximum Suppression (NMS)
Contour thinning by keeping only **local maxima** along the estimated gradient direction — produces **1‑pixel wide** edges before thresholding.

### 5) Colorized Gradient
RGB channels encode directional responses to visualize orientation:
- **R** ← horizontal (X)  
- **G** ← vertical (Y)  
- **B** ← diagonals (45°/135°)  
Mixing (e.g., **yellow** = R+G) reveals co‑occurring orientations; **white** indicates strong responses in all directions.

---

## 🖼️ Sample Results (see `resultat/`)

- `prewitt.jpg`, `sobel.jpg`, `kirsch.jpg` + `hist_*.jpg`  
- `magnitude_bidirectional.jpg`, `direction_bidirectional.jpg`  
- `magnitude_multidirectional_prewitt.jpg`, `direction_multidirectional_prewitt.jpg`  
- `seuillage_global.jpg`, `seuillage_hysteresis.jpg`  
- `suppressed_gradient.jpg`, `contour_colore.jpg`, `gradient_colore.jpg`

The **report** (`Rapport/rapport.md`) explains why **Sobel/Prewitt** tend to produce more detailed contours than **Kirsch**, even with multi‑directional combination, and analyzes color composition in the gradient visualization.

---

## ⚙️ Build & Run

### Prerequisites
- **C++17**, **CMake**
- **OpenCV 4.x**
- **Qt 5/6 (Widgets)**

### Configure & Build
```bash
cd code
mkdir -p build && cd build
cmake ..
make -j
```

### Run
```bash
# Qt GUI (includes tabs: Filtering, Bidirectional, Thresholding + sliders for hysteresis)
./QtOpencvExample
```
Inputs live in `image/`; outputs are saved to `resultat/`.

> If you prefer a headless flow, reuse the processing functions in `mywindow.cpp` from a console target.

---

## 🧠 Implementation Notes

- **Kernel normalization**: 3×3 integer masks are normalized (sum of abs) to float for consistent scaling.  
- **Multidirectional**: absolute-sum vs **max‑response** changes interpretability (dominant orientation).  
- **Hysteresis sliders**: `T_low` and `T_high` are exposed in the GUI for quick tuning.  
- **NMS**: applied on combined magnitude using local comparisons along the dominant direction.  
- **Display**: magnitude normalized to 8‑bit; direction mapped through **HSV→BGR** for visualization.

---

## 👨‍💻 Authors

**Alexandre COTTIER** — Master’s student in Computer Science *(ID3D)*, Université Claude Bernard Lyon 1  
**Anh Duy VU** — Co‑author

📍 Lyon, France  
🔗 GitHub: https://github.com/Mantador01  
🔗 LinkedIn: https://www.linkedin.com/in/alexandre-cottier-72ab20227/

---

## 📜 License

This work is for **educational and research purposes**.  
You may reuse and adapt the code with attribution.
