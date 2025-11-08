# Bicycle Model

Interactive single-track (bicycle) model for teaching and demonstrating basic vehicle lateral dynamics. The HTML app visualizes how classic linear bicycle-model assumptions translate into slip angles, yaw rates, lateral forces, and stability limits. It is intentionally lightweight so it can be dropped into a lecture, lab, or workshop without additional tooling.

> **Educational focus**  
> The model is meant for qualitative understanding and classroom use. It is *not* a validated vehicle dynamics simulator and should not be used to size components, certify control systems, or predict safety-critical behavior.

---

## Highlights
- Interactive sliders for mass, inertia, axle distances, cornering stiffnesses, forward speed, and steering angle.
- Real-time computation of steady-state slip angle β, yaw rate ψ̇, lateral acceleration, axle slip angles, and lateral tire forces.
- On-canvas visualization of the vehicle, vectors, and forces to make abstract variables tangible.
- Automatic warnings when user inputs exceed the valid range (e.g., speed beyond the linear stability limit).
- 100% client-side: open `bicycle_model.html` directly in a modern browser (Chrome, Firefox, Edge, Safari).

---

## Quick Start
1. Clone or download this repository.
2. Open `bicycle_model.html` with a modern browser. No build or server step is required.  
   - The page loads Tailwind CSS and Google Fonts from CDNs. For offline teaching, replace those links with locally hosted assets.
3. Adjust parameters with the sliders or number inputs in the left column.
4. Observe the visualization, numeric outputs, and warnings in the right-hand panels.

Tip: Mirror your screen or share the browser window during lectures. Students can experiment independently by sending them the static HTML file.

---

## Input Parameters

| Parameter | Symbol | Default | Range | Unit | Notes |
|-----------|--------|---------|-------|------|-------|
| Vehicle mass | `m` | 1500 | 500 – 3000 | kg | Lumped vehicle mass. |
| Yaw moment of inertia | `I_z` | 3000 | 1000 – 6000 | kg·m² | About the vertical axis through the CG. |
| CG to front axle | `l_f` | 1.2 | 0.5 – 2.5 | m | Also called the front lever arm. |
| CG to rear axle | `l_r` | 1.6 | 0.5 – 2.5 | m | Rear lever arm. |
| Front cornering stiffness | `c_f` | 80 000 | 20 000 – 150 000 | N/rad | Linearized tire stiffness. |
| Rear cornering stiffness | `c_r` | 80 000 | 20 000 – 150 000 | N/rad | Linearized tire stiffness. |
| Forward velocity | `v` | 15 | 0.1 – 60 | m/s | Constant longitudinal speed assumption. |
| Front steering angle | `δ` | 3 | –10 – 10 | ° | Small-angle input in degrees. |

All inputs are treated as steady-state constants; no transient effects are modeled.

---

## Output Variables

| Quantity | Symbol | Unit | What it shows |
|----------|--------|------|---------------|
| Body slip angle | `β` | ° | Difference between velocity vector and longitudinal axis. |
| Yaw rate | `ψ̇` | °/s | Steady-state yaw velocity. |
| Lateral acceleration | `a_y` | m/s² | Simple product `v * ψ̇`. |
| Front slip angle | `α_f` | ° | Local tire slip from linear tire model. |
| Rear slip angle | `α_r` | ° | Same for the rear axle. |
| Front lateral force | `F_y,v` | N | `c_f * α_f`. |
| Rear lateral force | `F_y,h` | N | `c_r * α_r`. |

If the requested speed exceeds the calculated critical speed or if numerical singularities occur, the app zeros the outputs and displays a warning banner.

---

## Model Background
- Based on the classical linear single-track model from Riekert & Schunck (1940).  
- Tires follow a linear cornering stiffness relationship (`F_y = c * α`).  
- The car moves at constant longitudinal speed with small slip angles and small steering inputs.  
- Roll dynamics, load transfer, compliance steer, saturation effects, and transient responses are intentionally omitted to keep the math transparent.

The code that implements the equations lives inside `bicycle_model.html` in the `calculateVehicleDynamics` function. It also reports the theoretical critical speed  
`v_crit = sqrt( (c_f * c_r * (l_f + l_r)^2) / (m * (l_f * c_f - l_r * c_r)) )`,  
which marks the stability limit of the linear model for the chosen parameters.

---

## Teaching & Demonstration Ideas
- **Concept introduction:** Move a single slider at a time while explaining how each parameter influences β and ψ̇.  
- **Sensitivity studies:** Ask students to match a target lateral acceleration by tuning `c_f`, `c_r`, or the lever arms.  
- **Stability discussion:** Increase speed until the warning appears to illustrate why understeer (`l_f * c_f > l_r * c_r`) matters.  
- **Controller exercises:** Use the outputs as references for simplified yaw-rate control derivations.  
- **Homework:** Have students rebuild the `calculateVehicleDynamics` equations in MATLAB/Python and validate against the HTML tool.

---

## Repository Layout
- `bicycle_model.html` – self-contained teaching app (HTML, CSS via Tailwind CDN, vanilla JS for logic).
- `README.md` – project documentation (this file).
- `LICENSE` – MIT License covering the code and assets.

---

## Extending the Model
- **Localization:** Replace German labels/strings in the HTML file if you teach in another language.
- **Custom styling:** Bundle Tailwind CSS locally (e.g., using `npx tailwindcss -i input.css -o output.css`) for offline use or custom themes.
- **Additional outputs:** Add transient approximations, understeer gradient, or transfer functions by expanding `calculateVehicleDynamics`.
- **Data export:** Hook the `currentOutputs` object to file downloads or websocket streams for lab experiments.

Pull requests that improve clarity, pedagogy, or numerical robustness are welcome.

---

## License & Attribution
- Code is released under the [MIT License](LICENSE).  
- The historic reference is: **Riekert, P., & Schunck, T. E. (1940). Über die Fahreigenschaften von Kraftfahrzeugen.**  
- Portions of the public-domain dedication quoted in the HTML footer acknowledge that the educational content is intended to be freely shared. Please keep the original attribution and contact details (`mschaefer@uni-wuppertal.de`) when redistributing.

---

## Support
Issues, suggestions, or reports of modeling mistakes are best sent via GitHub issues or directly to `mschaefer@uni-wuppertal.de`. Let us know how you use the tool in your classes—real-world feedback helps shape future teaching features.
