## 💻 `[Methodology]` Building Energy Modeling in CityBEM

CityBEM performs **transient building thermal modeling** to estimate heating, cooling, and total energy consumption for large urban building stocks.
This page provides a detailed explanation of the **physical models**, **mathematical formulations**, and **numerical structure** used in CityBEM.

---

## 1. Building Thermal Modeling Overview

CityBEM models each building as a **single thermal zone** with well-mixed indoor air.
A constant-air-volume (**CAV**) HVAC system is assumed to condition the air.

The model incorporates:

* Conduction through walls, roofs, floors, and thermal mass
* Glazing heat transfer
* Solar shortwave & longwave radiation
* Internal loads (occupant, lighting, equipment)
* Infiltration heat transfer
* HVAC heating & cooling power
* Coupling with PV and microclimate modules

<div align="center">
  <img src="../assets/building_thermal_modeling.png" alt="Building thermal modeling schematic" width="90%">
</div>

---

## 2. Conduction Heat Transfer Model

CityBEM uses a **1D transient conduction model** discretized into thermal nodes, typically solved using a finite-difference or thermal network method.

### 2.1 Governing Equations

The **energy balance** for an interior node $i$ at the next time step ($\tau+1$):

$$
C_i \frac{T_i^{\tau+1} - T_i^{\tau}}{\Delta t} =
\frac{T_{i-1}-T_i}{R_{i-1, i}}
+
\frac{T_{i+1}-T_i}{R_{i, i+1}}
+
S_i^{\tau}
$$

### Node thermal capacity

$$
C_i = \rho_i \,A_i \,\Delta x_i \,C_{p,i}
$$

### Element thermal resistance (Between nodes $i$ and $j$)

$$
R_{i, j} = \frac{\Delta x_i}{2 k_i A_i} + \frac{\Delta x_j}{2 k_j A_j}
$$

### Total layer resistance (Steady-State $R$-value)

$$
R_t = \frac{L}{k}
$$

### Variables

* **$C$**: Thermal heat capacity ($J/K$)
* **$\rho$**: Density ($kg/m^3$)
* **$C_p$**: Specific heat ($J/kg \cdot K$)
* **$R$**: Thermal resistance ($K/W$)
* **$k$**: Thermal conductivity ($W/m \cdot K$)
* **$L$**: Layer thickness ($m$)
* **$A$**: Cross-sectional area ($m^2$)
* **$S$**: Absorbed shortwave radiation/internal source ($W$)
* **$\Delta t$**: Time step ($s$)

---

## 3. Boundary Conditions

### 3.1 Outdoor-Facing Surfaces

The energy balance for the **exterior surface node $N$** includes: Conduction, Convection, Shortwave, and Longwave Radiation exchanges.

#### Shortwave absorption source term at the exterior surface node $N$

$$
S_{N,\mathrm{shd}} =
A_{\mathrm{sur}} \alpha_{\mathrm{sur}}
\left(
I_{\mathrm{db,tilt}} f_{\mathrm{shading}}
+ I_{\mathrm{sd,tilt}}
+ I_{\mathrm{gd,tilt}}
\right)
$$

### 3.2 Indoor-Facing Surfaces

The energy balance for the **interior surface node $1$** includes: Conduction, Convection to indoor air, and Longwave radiation exchange with thermal mass.

#### Heat exchange resistances to the indoors

Resistance due to convection to the indoor air $T_{\mathrm{air,in}}$:
$$
R_{\mathrm{air,in}} = \frac{1}{A_{\mathrm{sur}} h_{\mathrm{conv,in}}}
$$

---

## 4. Glazing Heat Balance Model

CityBEM uses a **two-node glazing model** for the outdoor pane ($\theta_{\mathrm{out}}$) and the indoor pane ($\theta_{\mathrm{in}}$).

### Outdoor glazing node (Steady-State Energy Balance $\approx 0$)

$$
h_{\mathrm{conv,out}} A_{\mathrm{glz}} (T_{\mathrm{air,out}}-\theta_{\mathrm{out}})
+ h_{\mathrm{rad,out}} A_{\mathrm{glz}} (\bar{T}_{\mathrm{sky/grd}}-\theta_{\mathrm{out}})
+ \frac{k_{\mathrm{glz}} A_{\mathrm{glz}}}{L_{\mathrm{glz}}}(\theta_{\mathrm{in}}-\theta_{\mathrm{out}})
+ S_{\mathrm{out}} = 0
$$

### Indoor glazing node (Steady-State Energy Balance $\approx 0$)

$$
h_{\mathrm{conv,in}} A_{\mathrm{glz}} (T_{\mathrm{air,in}}-\theta_{\mathrm{in}})
+ h_{\mathrm{rad,in}} A_{\mathrm{glz}} (T_{\mathrm{th.mass}}-\theta_{\mathrm{in}})
+ \frac{k_{\mathrm{glz}} A_{\mathrm{glz}}}{L_{\mathrm{glz}}}(\theta_{\mathrm{out}}-\theta_{\mathrm{in}})
+ S_{\mathrm{in}} = 0
$$

### Shortwave absorption (with shading)

$$
S_{\mathrm{in,shd}} = S_{\mathrm{out,shd}} =
\frac{1}{2} A_{\mathrm{glz}} \alpha_{\mathrm{glz}}
\left(
I_{\mathrm{db,tilt}} f_{\mathrm{shading}}
+ I_{\mathrm{sd,tilt}}
+ I_{\mathrm{gd,tilt}}
\right)
$$

---

## 5. Solar Energy Flow Through Glazing

The solar heat gain entering the indoor space (transmitted shortwave radiation):

$$
Q_{\mathrm{solar,glz}} =
A_{\mathrm{glz}} \, SHGC_{\mathrm{eff}}
\left(
I_{\mathrm{db,tilt}} f_{\mathrm{shading}}
+ I_{\mathrm{sd,tilt}}
+ I_{\mathrm{gd,tilt}}
\right)
$$

---

## 6. Total Building Heat Load

The net heat load $Q_t$ into the indoor air zone:

$$
Q_t = Q_{\mathrm{conv}} + Q_{\mathrm{inf}} + Q_{\mathrm{internal}} + Q_{\mathrm{solar,glz}}
$$

### 6.1 Conduction/Glazing heat gain/loss (Convective portion)

$$
Q_{\mathrm{conv}} =
\sum_{i}
A_{i} h_{\mathrm{conv,in},i}
\left(
T_{\mathrm{sur,in},i} - T_{\mathrm{air,in}}
\right)
$$

### 6.2 Infiltration

$$
Q_{\mathrm{inf}} =
F_{\mathrm{inf}} \rho_{\mathrm{air}} C_{p,\mathrm{air}}
\left(
T_{\mathrm{air,out}} - T_{\mathrm{air,in}}
\right)
$$

Volumetric infiltration flow rate:
$$
F_{\mathrm{inf}} = \frac{V_b \, ACH}{3600}
$$

### 6.3 Internal loads

$$
Q_{\mathrm{internal}} =
Q_{\mathrm{occupants}}
+ Q_{\mathrm{lighting}}
+ Q_{\mathrm{equipment}}
$$

---

## 7. Indoor Air Temperature Balance

The transient energy balance for the indoor air temperature $T_{\mathrm{air,in}}$:

$$
\rho_{\mathrm{air}} V_b C_{p,\mathrm{air}}
\frac{T_{\mathrm{air,in}}^{\tau+1} - T_{\mathrm{air,in}}^{\tau}}{\Delta t}
=
Q_t^{\tau}
+
\dot{M}_{\mathrm{HVAC}} C_{p,\mathrm{air}}
\left(
T_{\mathrm{sup}} - T_{\mathrm{air,in}}^{\tau}
\right)
$$

---

## 8. HVAC Power Consumption

### Heating

$$
PC_{\mathrm{HVAC,h}} =
\frac{Q_{\mathrm{HVAC,h}}}{\eta_h}
+ PC_{\mathrm{fan}}
$$

### Cooling

$$
PC_{\mathrm{HVAC,c}} =
\frac{Q_{\mathrm{HVAC,c}}}{COP}
+ PC_{\mathrm{fan}}
$$

### Total building power

$$
PC_{b,t} =
PC_{\mathrm{HVAC}}
+ PC_{\mathrm{lighting}}
+ PC_{\mathrm{equipment}}
$$

---

## 9. Summary Table (Appendix A)

A full mathematical summary is included in:

* Rayegan et al. (2024)
* Katal et al. (2023–2024)

---

## References

Rayegan, S., et al. 2024
Katal, A., et al. 2023–2024