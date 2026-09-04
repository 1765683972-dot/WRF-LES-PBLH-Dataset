# WRF Data

This dataset contains the WRF simulation inputs used for radiation-fog boundary-layer parameterization correction. The simulations contains four schemes:

- YSU — Yonsei University PBL scheme
- ACM2 — Asymmetric Convective Model, version 2
- QNSE — Quasi-Normal Scale Elimination scheme
- LES — Large Eddy Simulation

The data correspond to a single hourly WRF output time. The domain has a 3 km horizontal resolution, 39 vertical mass levels.

## Directory structure

Each scheme directory contains four NetCDF4 files:

```text
WRF/YSU/
├── thermodynamic.nc
├── cloud_phase.nc
├── dynamics_surface.nc
└── grid.nc
```

The ACM2, QNSE and LES directories have the same structure.

## Variables

### `thermodynamic.nc`

- `T`: air temperature (K)
- `P`: full pressure, calculated as `P + PB` (Pa)
- `Qv`: water-vapor mixing ratio (kg kg⁻¹)
- `RH`: relative humidity (%)

### `cloud_phase.nc`

- `Qc`: cloud-water mixing ratio (kg kg⁻¹)
- `Qr`: rainwater mixing ratio (kg kg⁻¹)
- `Qi`: ice mixing ratio (kg kg⁻¹)
- `Qs`: snow mixing ratio (kg kg⁻¹)

### `dynamics_surface.nc`

- `u`, `v`: horizontal wind components on the mass grid (m s⁻¹)
- `w`: vertical velocity on the mass grid (m s⁻¹)
- `W10`: 10 m wind speed (m s⁻¹)
- `HFX`: surface sensible heat flux (W m⁻²)
- `LH`: surface latent heat flux (W m⁻²)
- `H`: surface elevation (m)
- `S`: terrain slope (degrees)
- `TKE`: PBL turbulent kinetic energy (m² s⁻²), available for QNSE only

### `grid.nc`

Contains latitude/longitude coordinates and vertical-coordinate metadata, including `Times`, `XLAT`, `XLONG`, `ZNU`, and `ZNW`.

## Data processing

The wind components are destaggered from the original WRF grid to the mass grid. Temperature is reconstructed from WRF perturbation potential temperature and full pressure. Relative humidity and 10 m wind speed are derived from the corresponding WRF fields. Terrain slope is calculated from surface elevation.

All files use NetCDF4 with zlib compression and can be read with `netCDF4`, `xarray`, or other NetCDF-compatible software.
