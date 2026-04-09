
# 🔥 Laminar Flow Heat Transfer in a Circular Tube – COMSOL Multiphysics® 5.x

**COMSOL FEM | CFD | Heat Transfer | Thermal-Hydraulic Analysis**

A comprehensive 2D axisymmetric simulation of laminar airflow through a heated/cooled circular tube, performed using COMSOL Multiphysics® version 5.x. This project evaluates the heat transfer coefficient using both numerical computation and theoretical Nusselt number correlations (Graetz problem solution).

---

## 📋 Project Overview

This repository contains a complete COMSOL model that solves coupled fluid flow and heat transfer equations (conjugate heat transfer) for laminar flow in a pipe with constant wall heat flux or constant wall temperature. The model demonstrates thermal entrance region behavior and validates numerical results against classical correlations.

---

## 📐 Geometry Parameters

| Parameter | Value | Description |
|-----------|-------|-------------|
| **L** | 3 m | Tube length |
| **b** | 0.05 m | Tube diameter (D) |

---

## ⚙️ Material Properties (Air)

| Property | Value | Unit |
|----------|-------|------|
| Thermal conductivity (k) | ~0.026 | W/(m·K) |
| Density (ρ) | ~1.2 | kg/m³ |
| Heat capacity (Cp) | ~1005 | J/(kg·K) |
| Dynamic viscosity (μ) | ~1.8e-5 | Pa·s |

> Properties are temperature-dependent in the full model; nominal values at 293K shown.

---

## 🧪 Boundary Conditions

| Boundary | Condition | Value |
|----------|-----------|-------|
| Inlet (z=0) | Fully developed velocity profile | `U = 1.5*U_av*(1-4*(r/b)^2)` |
| Inlet temperature | Fixed | 283 K |
| Tube wall | Heat flux **OR** Fixed temperature | `qw = 10 W/m²` **or** `Tw = 293 K` |
| Outlet (z=L) | Pressure outlet | 0 Pa (gauge) |
| Axis (r=0) | Symmetry | Axial symmetry |

---

## 📊 Key Variables & Derived Quantities

| Variable | Expression | Unit | Description |
|----------|------------|------|-------------|
| **U** | `1.5*U_av*(1-4*(r/b)^2)` | m/s | Inlet velocity profile (laminar, parabolic) |
| **Tb** | `integrate(comp1.at2(r,z,2*pi*r*w*T),r,0,b/2) / integrate(comp1.at2(r,z,2*pi*r*w),r,0,b/2)` | K | Bulk mean temperature |
| **Ub** | `integrate(comp1.at2(r,z,2*pi*r*w),r,0,b/2) / (pi*(b/2)^2)` | m/s | Bulk mean velocity |
| **Tc** | `comp1.at2(0,z,T)` | K | Centerline temperature |
| **Pr** | `ht.Cp*spf.mu/ht.kmean` | — | Prandtl number |
| **Re_D** | `nitf1.rho*Ub*b/spf.mu` | — | Reynolds number |
| **Gz** | `b*Re_D*Pr/z * pi/4` | — | Graetz number (local) |
| **Nu_D** | `(1+(Gz/19.04/((1+(Pr/0.0207)^2/3)^1/2*(1+(Gz/29.6)^2)^1/3))^(3/2))^(1/3)*4.364*(1+(Gz/29.6)^2)^(1/6)` | — | Local Nusselt number (Gnielinski correlation for thermal entrance) |

---

## 📊 Results & Visualization

### 1. Temperature Field (2D Axisymmetric)

*Figure: Color contour of temperature distribution along the tube length. Red = warmer fluid near inlet/wall; Blue = cooler regions.*

The temperature field shows:
- Thermal boundary layer development from the inlet
- Gradual heating/cooling of the bulk fluid
- Fully developed temperature profile near the outlet for longer tubes

### 2. Bulk Temperature vs. Axial Position

*Figure: Line plot of Tb(z) showing exponential approach to wall temperature (constant Tw case) or linear increase (constant qw case).*

### 3. Local Nusselt Number Along Tube

*Figure: Nu_D(z) from COMSOL (numerical) vs. theoretical Graetz solution correlation.*

**Key observations:**
- Very high Nu_D near inlet (thermal entrance region)
- Decay to asymptotic value (4.364 for constant Tw, 4.364 for constant qw in fully developed laminar flow)
- Excellent agreement between simulation and correlation beyond z/D > 0.05

---

## 🔬 Physics Interpretation

**Governing equations solved:**
- Navier-Stokes (steady, laminar, incompressible)
- Continuity equation
- Energy equation: `ρ Cp (u·∇T) = ∇·(k ∇T)`

**Heat transfer mechanisms:**
- **Convection** dominates in the core flow
- **Conduction** dominates near the wall (thermal boundary layer)
- Bulk temperature rise due to wall heat flux or temperature difference

**Validation approach:**
- Compare numerically computed `h(z) = qw/(Tw - Tb(z))` with correlation-based Nusselt number

---

## 🔬 Real-World Applications

### Application 1: Heat Exchanger Design

**Problem:** Shell-and-tube heat exchangers require accurate predictions of outlet temperatures and pressure drop.

**How this simulation applies:**
- Inlet (283K) → cold fluid entering
- Wall heating (qw or Tw) → hot utility side
- Tb(z) determines required tube length for target outlet temperature

**Impact:** Reduces overdesign by 15–25%, saving material costs.

### Application 2: Electronics Cooling with Liquid Flow

**Problem:** Cold plates for IGBTs or CPUs must remove heat efficiently without hotspots.

**How this simulation applies:**
- Tube → cooling channel in cold plate
- Wall heat flux → heat from electronic component
- Laminar flow → typical in compact cooling loops

**Impact:** Predicts maximum component temperature, preventing thermal failure.

---

## 📁 Repository Contents
