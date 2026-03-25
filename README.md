# Gravitational Waves-Lensing Mock Catalog(GW-LMC)

This dataset contains simulated gravitational wave lensing events observed by the **['ET', 'CE']** network.
The simulation assumes a specific cosmology and lens population model (based on GWTC-4.0 catalog).
[![DOI](https://zenodo.org/badge/1127564141.svg)](https://doi.org/10.5281/zenodo.19212270)

All files share the `event_id` column.

## Categories Description
* **Double_Systems**: Strong lensing events with 2 detected images (or specific configuration).
* **Quad_Systems**: Strong lensing events with 3 or more detected images (from 4/5 images).
* **Subhalo_Events**: Events where a subhalo is present in the lens model.
* **Golden_Systems**: "Golden" events where all theoretical images are detected (3/3 or 5/5).
* **High_Magnification**: Events with max image magnification > 3.0 and SNR > 8.0.
* **Any_Detected_SNR8**: Events where **at least 1 image** has SNR > 8.0 (Standard detection threshold).
* **Any_Detected_SNR1**: Events where **at least 1 image** has SNR > 1.0 (Loose threshold for sub-threshold analysis).

## 1. File Structure Overview
The data is split into three CSV files per category (e.g., `Double_Systems`, `Quad_Systems`) to organize parameters logically:

1. `*_SourceParams.csv`: Physical properties of the GW source (Binary Black Hole/Neutron Star).
2. `*_LensParams.csv`: Detailed parameters of the lens model (Dark matter halos, galaxies, shear).
3. `*_ImageParams.csv`: Observable properties of the lensed images (Time delays, magnifications, SNR).

**Linking Key:** All files share a unique identifier column `event_id` (e.g., `0001`, `0002`) to link rows across the three files.

---

## 2. Parameter Dictionary

### Table 1: Source Parameters (`_SourceParams.csv`)
Describes the properties of the binary system before lensing.

| Column Name | Unit | Description |
| :--- | :--- | :--- |
| `event_id` | - | Unique identifier for the event. |
| `m1_det` | $M_\odot$ | Mass of primary compact object (Detector frame). |
| `m2_det` | $M_\odot$ | Mass of secondary compact object (Detector frame). |
| `m1_src` | $M_\odot$ | Mass of primary compact object (Source frame). |
| `m2_src` | $M_\odot$ | Mass of secondary compact object (Source frame). |
| `z_source` | - | Redshift of the source. |
| `dl_source` | Mpc | Luminosity distance to the source. |
| `gps_time` | s | Merger time at geocenter (GPS seconds). |
| `ra` | rad | Right Ascension of the source. |
| `dec` | rad | Declination of the source. |
| `psi` | rad | Polarization angle. |
| `theta_jn` | rad | Inclination angle between total angular momentum and line of sight. |
| `phase` | rad | Coalescence phase. |
| `a1`, `a2` | [0,1] | Dimensionless spin magnitudes. |
| `tilt1`, `tilt2` | rad | Tilt angles of the spins relative to orbital angular momentum. |
| `phi12` | rad | Azimuthal angle between the two spin vectors. |
| `phijl` | rad | Azimuthal angle of orbital angular momentum. |

### Table 2: Lens Model Parameters (`_LensParams.csv`)
Describes the lens system. The lens model is constructed from up to 5 components.

#### 2.1 General Properties
| Column Name | Unit | Description |
| :--- | :--- | :--- |
| `lens_einstein_radius` | arcsec | Einstein radius of the main lens. |
| `lens_max_sep` | arcsec | Maximum separation between lensed images. |
| `lens_flux_ratio` | - | Ratio of the second brightest image flux to the brightest. |
| `src_pos_x_arcsec` | arcsec | Source position X in the source plane (relative to lens center). |
| `src_pos_y_arcsec` | arcsec | Source position Y in the source plane. |
| `lens_type_flag` | code | Internal flag for lens galaxy type. |
| `is_subhalo` | bool | `True` if a subhalo is present in the system. |
| `subhalo_frac` | - | Fraction of mass in the subhalo (if present). |

#### 2.2 Component Details
Columns follow the naming convention `[component]_[parameter]`.
**Components:**
* `host_halo`: The main dark matter halo.
* `central_gal`: The central baryon galaxy.
* `shear`: External shear field.
* `subhalo`: Dark matter subhalo (values are -1 if not present).
* `satellite`: Satellite galaxy (values are -1 if not present).

**Parameters for each component:**
| Suffix | Description |
| :--- | :--- |
| `_type` | Model code: 1=NFW, 2=Hernquist, 3=Singular Isothermal Sphere/Shear. |
| `_z` | Redshift of the lens component. |
| `_mass` | Mass parameter (usually $10^{10} M_\odot$ or specific scale mass). |
| `_x`, `_y` | Position (arcsec) relative to the coordinate center. |
| `_e` | Ellipticity ($e = 1 - q$). **Exception:** For `shear`, this is Shear Magnitude ($\gamma$). |
| `_pa` | Position Angle (degrees). **Exception:** For `shear`, this is Shear Angle ($\theta_\gamma$). |
| `_param` | Additional shape parameter (e.g., Scale radius or Concentration). |

### Table 3: Image Observables (`_ImageParams.csv`)
Contains lists of properties for all images formed by the lens.
*Note: Columns starting with `img_` contain string representations of lists (e.g., `"[1.2, 0.5]"`).*

| Column Name | Unit | Description |
| :--- | :--- | :--- |
| `n_images_theory` | int | Total number of theoretical images (solutions to lens equation). |
| `n_images_detected` | int | Number of images passing the SNR threshold (SNR > 8.0). |
| `img_snrs` | - | List of Signal-to-Noise Ratios for each image. |
| `img_mags` | - | List of magnifications (signed, negative implies parity flip). |
| `img_delays_days` | days | List of time delays relative to the first arriving image. |
| `img_x_arcsec` | arcsec | List of X positions of images on the sky. |
| `img_y_arcsec` | arcsec | List of Y positions of images on the sky. |
| `img_kappa` | - | List of convergence ($\kappa$) values at image positions. |
| `img_gamma1` | - | List of shear component 1 ($\gamma_1$) at image positions. |
| `img_gamma2` | - | List of shear component 2 ($\gamma_2$) at image positions. |
| `img_kappa_gal` | - | List of convergence contribution from the galaxy component only. |

---
**Generation Info:**
* **Date:** 2026-01-04 13:43:31
* **Pipeline Version:** 7.0 (Post-processing)
