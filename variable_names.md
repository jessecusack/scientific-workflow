# Short form oceanographic variables

Long forms of names such as `potential_temperature` are always understandable but often awkwardly long for coding. Short form names are also usually comprehensible, but can sometimes lead to confusion e.g. `temp` could be temperature or a temporary variable. Here is my take on short variables names that can usually be understood if the code is well written such that it provides context for the variables. 


| Variable  | Short name | Notes |
|---|---|---|
| Time     | `time`, `datenum` | Use `datenum` in the case that it really is a MATLAB datenum. _Please_ don't use `t` which is easily confused for temperature.
| Latitude | `lat` |
| Longitude| `lon` |
| Depth | `d`, `depth` | In the ocean _this should be a positive number_.
| Height | `z` | Equal to negative depth. In the ocean _this should be a negative number_.
| Pressure | `p`, `P` |
| Bottom depth | `h`, `H` |
| Height above bottom | `hab`, `HAB` |
| In situ temperature | `t`, `T` | 
| Potential temperature | `pt`, `theta` |
| Conservative temperature | `CT`, `C_T` |
| Conductivity | `C` | 
| Practical salinity | `s`, `S`, `SP`, `S_P` |
| Absolute salinity | `SA`, `S_A` |
| Buoyancy frequency | `N`, `bfrq` |
| Buoyancy frequency squared | `N2`, `Nsq` |
| Eastward or x velocity | `u`, `U` `uvel` |
| Northward or y velocity | `v`, `V`, `vvel` |
| Upward or z velocity | `w`, `W`, `wvel` |
| Gradients in velocity | `dudx`, `dvdy`, `dwdz`, etc... | `u_x`, `u_y`, `u_z`, etc. could be confused with velocity but might be ok in some cases
| Potential vorticity | `PV`, `q`, `Q` | 
| Geostrophic potential vorticity | ? |
| Ertel potential vorticity | ? |
| Poor person's potential vorticity | ? |
| Dissipation rate of turbulent kinetic energy, $\varepsilon$ | `eps` |
| Dissolved oxygen concentration | `DO` |
| Chlorophyl concentration | `ch`, `chlr`, ? |
| Transmissivity | `trans` |
| Turbidity | `turb` |
| Kinetic energy | `KE`, `Ek`, `E_k` |
| Potential energy | `PE`, `Ep`, `E_p` |

# Confusing things to avoid

The most common confusing coding practises that should be avoided, according to me:
* **depth is not pressure**: Pressure [dbar] should never simply called or replaced by depth [m] (except for the quickest, dirtiest calculations). There is a difference between these quantities which can be larger than 1 % in the deep ocean (so, over 40 m at a pressure of 4000 dbar). This matters.
* **depth is not height**: Depth increases downwards so is positive in the ocean. Height decreases downwards and should be negative.
* **temp**: `temp` is so often used to mean 'temporary variable', it should not be used for temperature. 
* **time**: `t` and `T` are so frequently used for things like temperature or period that they should not be used for time. Just use `time`. 
