# AE Spiral Maker (v1.0.0)

[![Live Browser Simulator](https://img.shields.io/badge/Live%20Demo-AE%20ScriptUI%20Simulator-2c8cff?style=for-the-badge&logo=javascript)](https://work2survive.github.io/AE_JS_builder/?jsx=AE_Spiral_Maker.jsx&autobuild=true)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)
[![Adobe After Effects](https://img.shields.io/badge/Adobe%20After%20Effects-CS6%20--%20CC%202026%2B-9999FF?style=for-the-badge&logo=adobeaftereffects)](https://www.adobe.com/products/aftereffects.html)
[![Built with Antigravity](https://img.shields.io/badge/Built%20With-Antigravity-4285F4?style=for-the-badge&logo=google)](https://deepmind.google/)

**Live Browser Demo**: [https://work2survive.github.io/AE_JS_builder/?jsx=AE_Spiral_Maker.jsx&autobuild=true](https://work2survive.github.io/AE_JS_builder/?jsx=AE_Spiral_Maker.jsx&autobuild=true)

A procedural vector spiral generator for Adobe After Effects. Openly inspired by the online SVG spiral utility at [tiny-online.tools/svg-tools/svg-spiral-generator](https://tiny-online.tools/svg-tools/svg-spiral-generator), rebuilt from the ground up for motion designers with native After Effects Shape Layers, animatable live effect controls, and **native After Effects Stroke Taper integration** for independent start and end stroke thicknesses.

---
[![Watch the video](https://img.youtube.com/vi/Fc9rlHDSWjM/maxresdefault.jpg)](https://youtu.be/Fc9rlHDSWjM)

## 🎯 Key Capabilities

### 1. Start & End Stroke Thickness (Powered by Native AE Taper)
Unlike standard vector generators that output fixed-width strokes, **AE Spiral Maker** leverages After Effects' native Shape Layer **Stroke Taper** engine (`ADBE Vector Taper`, introduced in AE CC 2020 / v17.0+):
- **Start Stroke Thickness (px)**: Set the exact pixel stroke thickness at the center/inner origin of the spiral.
- **End Stroke Thickness (px)**: Set the exact pixel stroke thickness at the outer terminus tip.
- **Continuous Taper (Full Path)**: Automatically calculates the maximum base stroke width and links Taper Start Width %, End Width %, and Length % via expressions, ensuring a silky, uninterrupted thickness progression along the entire spiral length.
- **Directional Adaptation**:
  - Expanding Spiral ($T_{\text{start}} < T_{\text{end}}$): Narrows at the core and smoothly blossoms into a thick outer ribbon.
  - Needle / Whip Spiral ($T_{\text{start}} > T_{\text{end}}$): Starts bold and heavy at the center, tapering down to a razor-sharp point.
  - Uniform Stroke ($T_{\text{start}} = T_{\text{end}}$): Crisp, consistent vector stroke.
- **Manual Taper Lengths & Eases**: When custom tips are desired, uncheck Continuous Taper to freely adjust Start Length %, End Length %, and Start/End Ease profiles (-100% to +100%).

### 2. Five Mathematical Spiral Formulations
Choose between 5 mathematically distinct spiral algorithms:
1. **Archimedean Spiral** ($r = r_{\text{start}} + \frac{r_{\text{end}} - r_{\text{start}}}{\theta_{\text{max}}} \cdot \theta$):
   Constant radial pitch between consecutive windings. Ideal for vinyl records, coils, watch springs, and geometric op-art.
2. **Logarithmic Spiral** ($r = r_0 \cdot e^{k\theta}$):
   Exponential growth where coil spacing expands outward. Models natural phenomena such as nautilus shells, whirlpools, and tropical storms.
3. **Golden Spiral** (Fibonacci Ratio $\phi \approx 1.61803398875$):
   Logarithmic spiral expanding by the golden ratio, providing harmonious classical proportions.
4. **Fermat's Spiral** ($r = r_{\text{start}} + (r_{\text{end}} - r_{\text{start}}) \cdot \sqrt{u}$):
   Parabolic growth model famous for sunflower seed phyllotaxis and galactic core patterns.
5. **Hyperbolic Spiral** ($r \propto 1 / \theta$):
   Reciprocal spiral curving gracefully outward into wide cosmic funnels.

### 3. Analytical Cubic Bézier Tangents
Every vertex along the spiral curve is evaluated with exact mathematical derivatives ($\frac{dr}{d\theta}$, $\frac{dx}{d\theta}$, $\frac{dy}{d\theta}$), generating analytical cubic Bézier tangent handles ($h = \frac{4}{3}\tan(\Delta\theta/4)$). This eliminates facet kinks, producing pure continuous curvature with minimal vertex counts.

### 4. Live Expression Rig & Animation
- **Parametric Sliders in Effect Controls**: All geometric controls (Turns, Radii, Start Angle, Direction, Scale X/Y, Start/End Thickness, Taper Lengths/Eases) are exposed directly on the layer in After Effects' Effect Controls panel (`ADBE Effect Parade`).
- **Animatable**: Keyframe any parameter over time to create hypnotic morphing, spinning, or expanding spirals.
- **Built-in Trim Paths**: Includes a native Trim Paths modifier with Start, End, and Offset properties for instant write-on or draw-off animations.
- **Update Selected Spiral Layer**: Allows updating an existing spiral layer in-place from the panel without recreating the layer.

### 5. Presets Engine (`AE_spiral_maker/presets.json`)
Ships with 8 production-ready factory presets and loads from an external JSON file next to the script:
- 🌀 **Classic Archimedean**: Balanced 4-turn spiral with 2px to 24px taper in vibrant electric cyan.
- 🐚 **Nautilus Shell**: Logarithmic growth, 3.2 turns, 3px to 36px organic taper in ocean teal.
- 🌌 **Golden Spiral**: Fibonacci ratio curve, 2.5 turns, 4px to 32px taper in cosmic gold.
- 💿 **Vinyl Record Groove**: 14 uniform turns, inner spindle hole, 2.5px fine groove in graphite silver.
- 🌻 **Sunflower Core**: Fermat parabolic distribution, 6 turns, solar yellow.
- 🌪️ **Hypnotic Swirl**: Counter-clockwise reverse taper (38px center down to 2px tip) in electric magenta.
- ✒️ **Zen Calligraphy**: Heavy 42px center tapering to a 1px needle point with elliptical aspect distortion.
- ⚡ **Cyber Vortex**: Electric neon pulse with custom stroke profile.

Edit `presets.json` freely or add custom presets, then click the reload button (**🔄**) in the panel to instantly refresh without restarting After Effects!

---

## 📐 Mathematical Formulation

For parameter $u \in [0, 1]$ across total angle $\Theta = 2\pi \cdot \text{Turns}$, with initial rotation $\alpha_0$ and direction $s \in \{+1, -1\}$:

$$\phi(u) = \alpha_0 + s \cdot (u \cdot \Theta)$$

### Radius Functions $r(u)$ & Derivatives $\frac{dr}{d\theta}$:

| Spiral Type | Radius Equation $r(u)$ | Radial Derivative $\frac{dr}{d\theta}$ |
| :--- | :--- | :--- |
| **Archimedean** | $r(u) = r_0 + (r_1 - r_0) \cdot u$ | $\frac{r_1 - r_0}{\Theta}$ |
| **Logarithmic** | $r(u) = r_0 \cdot \exp\left(u \cdot \ln\frac{r_1}{r_0}\right)$ | $\frac{\ln(r_1/r_0)}{\Theta} \cdot r(u)$ |
| **Golden Spiral** | $r(u) = r_0 \cdot \exp\left(u \cdot \ln\frac{r_1}{r_0}\right)$ | $\frac{\ln(r_1/r_0)}{\Theta} \cdot r(u)$ |
| **Fermat** | $r(u) = r_0 + (r_1 - r_0) \cdot \sqrt{u}$ | $\frac{r_1 - r_0}{\Theta \cdot 2\sqrt{u}}$ |

### Tangent Vector Conversion:
$$\mathbf{v} = \left[ \left(\frac{dr}{d\theta}\cos\phi - r\sin\phi \cdot s\right) \cdot S_x, \; \left(\frac{dr}{d\theta}\sin\phi + r\cos\phi \cdot s\right) \cdot S_y \right]$$

$$\text{inTangent} = -h \cdot \mathbf{v}, \quad \text{outTangent} = +h \cdot \mathbf{v}, \quad \text{where } h = \frac{4}{3}\tan\left(\frac{\Delta\theta}{4}\right)$$

---

## 🛠️ Installation & Setup

1. **Download the Script**:
   Save [`AE_Spiral_Maker.jsx`](AE_Spiral_Maker.jsx) and the [`AE_spiral_maker/`](AE_spiral_maker/) presets folder to your computer.
2. **Copy to After Effects Scripts Folder**:
   - **Windows**: `C:\Program Files\Adobe\Adobe After Effects <Version>\Support Files\Scripts\ScriptUI Panels\`
   - **macOS**: `/Applications/Adobe After Effects <Version>/Scripts/ScriptUI Panels/`
3. **Launch After Effects**:
   - Open After Effects.
   - Go to **Window > AE_Spiral_Maker.jsx** to dock the panel anywhere in your workspace.
   - Alternatively, run via **File > Scripts > Run Script File...** to open as a floating palette.

---

## 💻 Live Web Simulator

You can test and preview **AE Spiral Maker** interactively in the browser without launching After Effects:
👉 **[Open Live Simulator](https://work2survive.github.io/AE_JS_builder/?jsx=AE_Spiral_Maker.jsx&autobuild=true)**

---

## 💖 Support / Sponsor

If you find this tool helpful and want to support its ongoing development and maintenance:

[![Sponsor work2survive](https://img.shields.io/badge/Sponsor-%E2%9D%A4-ea4aaa?style=for-the-badge&logo=github)](https://github.com/sponsors/work2survive)

Every contribution helps keep these open-source After Effects tools free, maintained, and continuously improved for the motion design community!

---

## 📄 License

This project is licensed under the [MIT License](LICENSE) - see the LICENSE file for details.

Developed & architected with **Google DeepMind Antigravity**. Co-authored by **work2survive**.
