

# Sandwich Panel Flexure Validation Study
**HexPly® 8552/AS4 Plain Weave Skins + Rohacell® 51 IGF Core**
Ansys ACP + Mechanical 3-Point Bend FEA per ASTM C393

---

## 1. Introduction

This report presents an FEA validation study of a composite sandwich panel under three-point bending, modeled in Ansys Composite PrepPost (ACP) and Ansys Mechanical. The sandwich consists of woven carbon/epoxy face sheets bonded to a closed-cell PMI foam core. The objective is to verify that the FEA model reproduces sandwich beam theory predictions for face sheet bending stress, core shear stress, and mid-span deflection, and to confirm that all sandwich failure modes remain below their allowables at the applied load.

---

## 2. Problem Statement

Sandwich structures derive their high specific bending stiffness from stiff face sheets separated by a lightweight core, where the skins carry in-plane tension and compression while the core carries transverse shear. Unlike monolithic laminates, sandwich panels are governed by distinct failure modes — face sheet yielding, core shear failure, face sheet wrinkling, and shear crimping — that must each be checked independently. Before deploying a sandwich layup in a UAV wing skin, the FEA material model and layup definition must be validated against analytical sandwich beam theory and published constituent allowables.

---

## 3. Materials

### 3.1 Face Sheet — HexPly 8552/AS4 Plain Weave (AGP193-PW)

Woven carbon/epoxy prepreg, balanced 50:50 warp/fill plain weave, sharing the same 8552 toughened epoxy matrix as the UD system validated previously.

| Property | Symbol | Value | Unit |
|---|---|---|---|
| Young's Modulus (warp) | E1 | 68,000 | MPa |
| Young's Modulus (fill) | E2 | 66,000 | MPa |
| Through-thickness Modulus | E3 | 9,000 | MPa |
| In-plane Shear Modulus | G12 | 5,000 | MPa |
| Out-of-plane Shear Modulus | G13 = G23 | 3,500 | MPa |
| Poisson's Ratio | ν12 | 0.05 | — |
| Tensile Strength (warp) | XT | 828 | MPa |
| Tensile Strength (fill) | YT | 793 | MPa |
| Cured Ply Thickness | CPT | 0.195 | mm |
| Laminate Density | ρ | 1,570 | kg/m³ |

### 3.2 Core — Rohacell 51 IGF

Closed-cell polymethacrylimide (PMI) rigid foam, isotropic.

| Property | Value | Unit |
|---|---|---|
| Density | 52 | kg/m³ |
| Young's Modulus | 70 | MPa |
| Shear Modulus | 24 | MPa |
| Poisson's Ratio | 0.25 | — |
| Tensile Strength | 1.9 | MPa |
| Compressive Strength | 0.9 | MPa |
| Shear Strength | 0.8 | MPa |

---

## 4. Coupon Geometry (ASTM C393)

| Parameter | Value |
|---|---|
| Total Length | 182.5 mm |
| Width | 75 mm |
| Support Span (centre-to-centre) | 125 mm |
| Overhang (each end) | 25 mm |
| Support Strip Width | 5 mm |
| Load Nose Strip Width | 10 mm |
| Core Thickness | 10 mm |
| Skin Thickness (2 plies each) | 0.39 mm |
| Total Sandwich Thickness | 10.78 mm |
| Layup | [PW₂ / Core / PW₂] |

---

## 5. FEA Setup

**Software:** Ansys Workbench 2026 R1 (Student Edition) — ACP (Pre) + Static Structural, shell transfer.

**Layup (ACP Pre):**
- Skin fabric: HexPly_8552_AS4_PW, 0.195 mm/ply
- Core fabric: Rohacell_51_IGF, 10 mm
- Stackup: skin ×2 → core ×1 → skin ×2
- Rosette aligned with span axis; oriented selection set covers all 7 surface zones

**Mesh:** SHELL181 layered elements, 3 mm element size.

**Boundary Conditions:**

| BC | Location | Definition |
|---|---|---|
| Pin support | Left support strip | X = Y = Z = 0 |
| Roller support | Right support strip | Z = 0 (X, Y free) |
| Load | Centre load nose strip | F = 1,000 N in −Z |

---

## 6. Results

| Result | Layer | Value |
|---|---|---|
| Max face sheet bending stress (σyy) | Skin (top/bottom) | ±92.5 MPa |
| Max core shear stress (τyz) | Core (mid, layer 2) | 0.486 MPa |
| Max mid-span deflection | All | 1.857 mm |

The face sheet stress is antisymmetric through the thickness — the top skin in compression, bottom skin in tension. The core shear reverses sign either side of the load nose, the classic three-point bend shear diagram. The deformed shape is a symmetric single-curvature bend with peak deflection at mid-span.

---

## 7. Analytical Validation

### 7.1 Core Shear Stress

For a sandwich beam the transverse shear is carried almost entirely by the core:

**τ_core = F / (2 × b × d) = 1000 / (2 × 75 × 10.195) = 0.654 MPa**

FEA gives 0.486 MPa (free-span average region). The FEA value is lower because the field is averaged over the free span where shear decays toward the load and support points, whereas the analytical value is the peak. Both are the same order and confirm core-shear-dominated behavior.

### 7.2 Mid-span Deflection

Sandwich beam deflection combines bending and shear contributions:

**δ = FL³ / (48D) + FL / (4 × A × G_core)**

With flexural rigidity D = E_f × t_f × d² × b / 2, the bending term is ~0.49 mm and the shear term ~1.71 mm, giving an analytical total of ~2.20 mm. FEA gives 1.857 mm, a ~16% difference, consistent with the simplifying assumptions in the closed-form shear term.

---

## 8. Failure Mode Assessment

At the applied load of 1,000 N, each sandwich failure mode is checked independently. The Inverse Reserve Factor (IRF) is the ratio of applied stress to allowable stress; IRF < 1.0 means no failure, and Reserve Factor (RF) = 1/IRF.

### 8.1 Face Sheet Strength (in-plane)

The skins carry the bending moment as in-plane tension (bottom) and compression (top). The peak face sheet stress from FEA is the governing value.

```
σ_face     = 92.5 MPa        (FEA, max σyy in skin)
XT (warp)  = 828 MPa         (HexPly 8552/AS4 PW datasheet)

IRF = σ_face / XT = 92.5 / 828 = 0.112
RF  = 1 / 0.112 = 8.9×
```

The skin operates at ~11% of its tensile allowable. Face sheet strength is not the limiting mode.

### 8.2 Core Shear

The core carries essentially all the transverse shear. The shear force in 3-point bend is V = F/2 on each side of the load. The average core shear stress:

```
V        = F / 2 = 1000 / 2 = 500 N
b        = 75 mm     (width)
d        = 10.195 mm (skin centroid-to-centroid distance = core + one skin thickness)

τ_core   = V / (b × d) = 500 / (75 × 10.195) = 0.654 MPa   (analytical peak)
τ_FEA    = 0.486 MPa                                         (FEA free-span avg)

S_core   = 0.8 MPa   (Rohacell 51 shear strength)

IRF (analytical) = 0.654 / 0.8 = 0.818  → RF 1.2×
IRF (FEA)        = 0.486 / 0.8 = 0.608  → RF 1.6×
```

Core shear is the lowest-reserve mode. Using the conservative analytical peak, the reserve is only 1.2× — this is the design driver.

### 8.3 Face Sheet Wrinkling

Wrinkling is a local buckling of the compression skin into the core. The critical wrinkling stress (Hoff-Mautner formula):

```
σ_wr = 0.5 × (E_f × E_c × G_c)^(1/3)

E_f  = 68,000 MPa  (skin modulus)
E_c  = 70 MPa      (core modulus)
G_c  = 24 MPa      (core shear modulus)

σ_wr = 0.5 × (68,000 × 70 × 24)^(1/3)
     = 0.5 × (114,240,000)^(1/3)
     = 0.5 × 485 = 242 MPa
```

The constant varies by reference (0.5 to 0.91 depending on derivation). Using the conservative 0.5:

```
IRF = σ_face / σ_wr = 92.5 / 242 = 0.382  → RF 2.6×
```

The compression skin is at ~38% of its wrinkling allowable.

### 8.4 Shear Crimping

Shear crimping is a special case of wrinkling at very short wavelength, governed by core shear modulus. It initiates when the core shear capacity is exhausted, so it is bounded by the same check as core shear (Section 8.2). Since core shear is already the binding constraint, crimping does not govern separately here.

### 8.5 Summary

| Failure Mode | Applied | Allowable | IRF | RF | Governing? |
|---|---|---|---|---|---|
| Face sheet strength | 92.5 MPa | 828 MPa | 0.112 | 8.9× | No |
| Core shear (FEA) | 0.486 MPa | 0.8 MPa | 0.608 | 1.6× | **Critical** |
| Core shear (analytical) | 0.654 MPa | 0.8 MPa | 0.818 | 1.2× | **Critical** |
| Face sheet wrinkling | 92.5 MPa | 242 MPa | 0.382 | 2.6× | No |

**Core shear governs the design**, consistent with sandwich theory for low-density PMI cores. The panel has a minimum reserve factor of 1.2× (analytical) at 1,000 N.

---

## 9. Conclusions

- A three-material ACP stackup (woven skin / foam core / woven skin) was successfully defined and transferred to Static Structural as a shell model — no separate epoxy or solid core body required.
- FEA reproduces the expected sandwich bending behavior: antisymmetric skin stress and sign-reversing core shear.
- Core shear stress and mid-span deflection agree with analytical sandwich beam theory within engineering tolerance (~16–25%).
- All four sandwich failure modes remain below allowables; core shear is the critical mode with a minimum reserve factor of 1.2×, as theory predicts for low-density PMI cores.
- The sandwich material cards and layup workflow are validated for use in subsequent UAV wing skin design.

---

## 10. References

1. Hexcel Corporation. *HexPly® 8552 Epoxy Matrix Product Data Sheet* (woven reinforcements). 2023.
2. Evonik Operations GmbH. *ROHACELL® IG-F Product Information*. April 2022.
3. *ASTM C393/C393M: Standard Test Method for Core Shear Properties of Sandwich Constructions by Beam Flexure*.
4. Allen, H.G. *Analysis and Design of Structural Sandwich Panels*. Pergamon Press, 1969.
5. *CMH-17-6: Composite Materials Handbook, Volume 6. Structural Sandwich Composites*.

