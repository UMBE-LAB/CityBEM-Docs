# Rooftop PV in CityBEM

## What Does CityBEM Model?

- One-diode crystalline module physics  
- Dynamic temperature + irradiance dependencies  
- Parameter extraction from real manufacturer STC data  
- Transient MPP (maximum power point) tracking 
- Series–parallel array configuration at rooftop scale 
- Rooftop layout design with realistic spacing + self-shading rules  
- End-to-end city-scale PV workflow linking geometry, shading, temperature, and electrical power  
- Transparent documentation of assumptions and limitations

CityBEM includes a **high-fidelity rooftop photovoltaic (PV) engine** built for city-scale, transient simulation.  
This module combines rigorous semiconductor physics with practical rooftop design logic to deliver realistic PV performance across thousands of buildings.

At its core, the framework uses a **physics-based one-diode model**, capturing how crystalline silicon modules respond to real operating conditions rather than relying on fixed efficiencies or simplified curves. The result is a PV workflow that is accurate, scalable, and fully integrated with building energy dynamics.

---

## 1. PV Model in CityBEM

CityBEM uses a one-diode, four-parameter PV model—a widely validated formulation that reproduces the real electrical behavior of crystalline silicon modules.  
The model links the module’s current–voltage characteristics to irradiance, cell temperature, diode saturation effects, and internal resistances, enabling a physically grounded representation of PV performance.

<span style="font-size: 1.2em;">What this enables</span><br>

Thanks to this physics-based foundation, CityBEM can capture:

- low-light and variable irradiance performance  
- rapid temperature-driven efficiency changes  
- partial shading behavior  
- high-temperature derating  
- realistic, hour-by-hour performance across the entire year

Such detail is essential for **urban PV potential analysis**, where rooftop geometry, shading, and operating conditions differ widely across thousands of buildings.

<figure markdown>
  ![One-diode PV model schematic](assets/pv_one_diode_model.png){ width="60%" loading=lazy }
  <figcaption>One-diode equivalent circuit of a crystalline PV module</figcaption>
</figure>

---

## 2. Variables

| Symbol | Meaning | Units |
|--------|---------|--------|
| \(I\) | Terminal current | A |
| \(V\) | Terminal voltage | V |
| \(I_L\) | Photocurrent (light-generated) | A |
| \(I_O\) | Diode reverse saturation current | A |
| \(R_s\) | Series resistance | Ω |
| \(\gamma\) | Diode voltage factor | dimensionless |
| \(T_c\) | Cell temperature | K |
| \(\phi\) | Plane-of-array irradiance | W·m⁻² |
| \(A_{PV}\) | Module area | m² |
| \(N_S\) | Modules in series | – |
| \(N_P\) | Parallel strings | – |

---

## 3. One-Diode Model

The one‑diode four-parameter model is widely accepted due to its balance of physical detail and computational efficiency, making it ideal for urban-scale transient simulations.

<span style="font-size: 0.9em;">Current–Voltage Relation</span><br>

\[
I = I_L - I_D
\]

<span style="font-size: 0.9em;">Diode current:</span><br>

\[
I_D = I_O \left[ \exp\left(\frac{q(V + I R_s)}{\gamma k T_c}\right) - 1 \right]
\]

Each equation is kept separate for proper Markdown rendering.

---

<span style="font-size: 1.2em;">Temperature & Irradiance Dependence</span><br>

<span style="font-size: 0.9em;">Diode current:</span><br>

\[
I_L = 
\left(\frac{\phi}{\phi_{\text{ref}}}\right)
\left(I_{L,\text{ref}} + \mu_{Isc}(T_c - T_{c,\text{ref}})\right)
\]

<span style="font-size: 0.9em;">Reverse saturation current:</span><br>

\[
I_O =
I_{O,\text{ref}}
\left(\frac{T_c}{T_{c,\text{ref}}}\right)^3
\exp\left[
\frac{q\,\varepsilon_G}{kA}
\left(
\frac{1}{T_{c,\text{ref}}}
-
\frac{1}{T_c}
\right)
\right]
\]

<span style="font-size: 0.9em;">where</span><br>

\[
A = \frac{\gamma}{N_{CS}}
\]

---

## 4. Processing Manufacturer Data

CityBEM extracts four **reference model parameters**  
\(
I_{L,\text{ref}},\; I_{O,\text{ref}},\; \gamma,\; R_s
\)  
from standard manufacturer datasheet values:

- \(I_{sc,\text{ref}}, V_{oc,\text{ref}}\)  
- \(I_{mp,\text{ref}}, V_{mp,\text{ref}}\)  
- \(\mu_{Isc}, \mu_{Voc}\)  
- reference cell temperature and irradiance

!!! info "Reference Parameter Initialization"
    These parameters are determined **once at startup**, forming a high-fidelity electrical signature of the PV module.  
    CityBEM then uses this signature to compute **dynamic, timestep-level PV performance** as irradiance and temperature vary throughout the simulation.

A nonlinear system is solved at short-circuit, open-circuit, and MPP.

<span style="font-size: 1.2em;">Overview of the Strategy:</span><br>

1. Start with a candidate \(R_s\).  
2. Solve the three diode equations at SC, OC, and MPP.  
3. Compute the model-predicted voltage temperature coefficient.  
4. Adjust \(R_s\) until the model prediction matches the datasheet value.

---

## 5. Maximum Power Point (MPP)

CityBEM solves the MPP dynamically at each timestep, enabling time‑varying PV efficiency, unlike constant‑efficiency PV models.

---

<span style="font-size: 1.2em;">Cell Temperature Model (NOCT-based)</span><br>

\[
T_c = T_{\text{out}} +
\frac{\phi \tau\alpha_{\text{ave}}}{U_L}
\left(1 -
\frac{\eta_{\text{PV}}}{\tau\alpha_{\text{ave}}}
\right)
\]

with:

\[
\eta_{\text{PV}} =
\frac{I_{mp,\text{ref}} V_{mp,\text{ref}}}
{\phi_{\text{ref}} A_{PV}}
\]

\[
U_L = 
\frac{\phi_{\text{NOCT}} \tau\alpha_{\text{ave}}}
{T_{c,\text{NOCT}} - T_{\text{amb,NOCT}}}
\]

---

<span style="font-size: 1.2em;">Pinpoint MPP</span><br>

\[
\frac{dP}{dV} = V\frac{dI}{dV} + I = 0
\]

CityBEM solves this using Newton–Raphson iteration at each time step as the conditions change.

---

## 6. PV Array Scaling
 
<span style="font-size: 1.2em;">From Single PV to Network Scaling</span><br>

\[
I_{L,\text{array}} = N_P I_L
\]

\[
I_{O,\text{array}} = N_P I_O
\]

\[
\gamma_{\text{array}} = N_S \gamma
\]

\[
R_{s,\text{array}} = \frac{N_S}{N_P} R_s
\]

<span style="font-size: 1.2em;">Explanation</span><br>

  Series PV panels (N<sub>S</sub>):<br>  
    → same current, voltages add: \(\gamma\) will be scales with \(N_S\).

  Parallel PV panels (N<sub>P</sub>):<br>
    → same voltage, currents add: \(I_L\) and \(I_O\) will be scaled with \(N_P\).

  Combined effect:<br>
    → series resistance of each string adds; parallel strings reduce total resistance.

---

## 7. Rooftop PV Arrangement

<figure markdown>
  ![PV self-shading geometry](assets/pv_self_shading.png){ width="80%" loading=lazy }
  <figcaption>PV self-shading geometry: tilt, module height, and row spacing</figcaption>
</figure>

Rooftop PV layout in this framework follows industry standards and practical design guidelines, ensuring that modules within the same array do not significantly shade each other. This configuration is determined once during the design stage.

CityBEM uses the setback ratio (SBR) to define the recommended spacing between PV module rows (as shown in the above figure):

\[
\text{SBR} = \frac{b}{a}
\]

\(a\): effective module height (m)  
\(b\): horizontal row spacing (m)

Typical design values:

- **SBR ≈ 2** in sunnier, lower-latitude climates  
- **SBR ≈ 3** in high-latitude regions (e.g., Canada)

Using a higher SBR reduces inter-row shading while reflecting realistic rooftop packing densities used in practice.


<span style="font-size: 1.2em;">PV array design logic</span><br>
For each building rooftop, the framework applies a practical and standards-based PV design, incorporating:

- recommended spacing to minimize self shading  
- configuring modules in series and parallel based on standards
- voltage and current limits for crystalline PV systems  
- maximizing extractable power based on best-practice layout

This approach provides a realistic and engineering-sound rooftop PV configuration for large-scale urban simulations. 

---

## 8. Inputs and Outputs

<span style="font-size: 1.2em;">Inputs</span><br>

- PV panel manufacture datasheet
- PV panel tilt, azimuth angles
- roof geometry, usable area  
- outdoor irradiance, temperature 

<span style="font-size: 1.2em;">Outputs</span><br>

- PV power output (**\(P_{PV}(t)\)**), current (**\(I_{mp}(t)\)**), and voltage (**\(V_{mp}(t)\)**)
- building‑level and city‑level aggregated energy yield  

---

## 9. Urban Scale Rooftop PV

CityBEM benefits from a PV model that is accurate, dynamic, inter-building shading-aware, and fast for city-scale UBEM simulations.  

<span style="font-size: 1.2em;">:material-beaker-outline: Physics-based</span><br>
Captures semiconductor behavior instead of constant-efficiency assumptions, including diode effects, series resistance, and temperature dependence.

<span style="font-size: 1.2em;">:material-timer-cog-outline: Dynamic</span><br>
Module efficiency updates every timestep based on irradiance and cell temperature—consistent with transient building simulations in CityBEM.

<span style="font-size: 1.2em;">:material-lan: Scalable</span><br>
Applies cleanly across all levels: PV panel → PV network → rooftop → district → entire city.

<span style="font-size: 1.2em;">:material-weather-sunny-alert: Shading-aware</span><br>
Shading reduces irradiance, which directly feeds into the PV model, producing physically correct power reductions.

<span style="font-size: 1.2em;">:material-speedometer: Fast</span><br>
Computationally efficient for thousands of buildings, long time series, and multiple retrofitting scenarios.

The figure below summarizes the city-scale PV workflow implemented in CityBEM.

<figure markdown>
  ![City-scale PV workflow](assets/pv_workflow_cityscale.png){ width="100%" loading=lazy }
  <figcaption>City-scale PV modeling workflow in CityBEM</figcaption>
</figure>