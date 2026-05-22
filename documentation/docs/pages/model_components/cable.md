# CABLE

This is a work in progress!

The [CABLE land model](https://github.com/CABLE-LSM/CABLE) forms the land component of the ACCESS family of climate and Earth System models. There are two major subcomponents: CABLE which provides the biophysical information ACCESS requires each time step, and CASA which provides the terrestrial carbon cycle information for ACCESS-ESM. CABLE is carefully linked into the atmospheric component [(UM)](./um.md) via the native land model of the UM [(JULES)](https://github.com/ACCESS-NRI/JULES). 

## CABLE in ACCESS-AM3
All ACCESS simulations require the use of [JULES](./jules.md) or CABLE if only to provide lower boundary conditions on the atmosphere.
CABLE links into UM/JULES within ACCESS-AM3 in multiple places, across two categories - technical and scientific.

### Technical coupling
Within an AM3 simulation (typically a simulated month or less) there are 4 key technical connections
 1. configuration read - both [JULES](./jules.md) and CABLE namelists are read.  These are editable (with care) in the rose-suite for the experiment.  Edits can also be made to specify which outputs are requested.
 2. variable declaration/allocation - given the input configuration additional CABLE specific variables are created for use.
 3. restart read/write - CABLE specific variables are read from/written to the restart file.  The restart and input configuration must match - automated checking of this within the rose-suite is a work in progress.
 4. output writing - at various points through the UM timestep variables are passed to STASH for accumulation/averaging through time, then written to the output files.

 CABLE variables are *always* output via a UM/JULES field (e.g. `fld_s03i540` for transpiration) - additional UM/JULES output fields have been created where necessary to accommodate structural differences between CABLE and JULES. Conversion to CF-compliant outputs is a separate step in the workflow.  Most land outputs that are available from ACCESS have been correctly populated with CABLE values [(diagnostics?)](../diagnostics/index.md) - or have been noted as not applicable.  If you find an error, or have a question as to whether there is an error, please alert the ACCESS-NRI via the [ACCESS-hive forum](https://forum.access-hive.org.au/).

 Here? Most land output from ACCESS can be provided on subdiurnal timescales however all the carbon cycle fluxes, including gross primary productivity (GPP), net primary productivity (NPP), autotrophic and heterotrophic respiration should be viewed as inaccurate on timescales of less than 1 day.       

![Structure 1](../../assets/structure1.jpg "Structure 1")
Legend:
- BLUE is UM code
- BEIGE is JULES code
- BROWN is JULES-CABLE coupling code (no science)
- GREEN is CABLE science code and JULES-CABLE coupling code (where necessary)
- DIAGONAL LINES indicate links to output via STASH

### Scientific coupling
 Within a model time step there are 4 principal scientific connection points between CABLE and UM/JULES
 1. `surf_couple_radiation`: evaluates the land contributions to the surface (4-band) albedos.
 2. `surf_couple_explicit`:  estimates the land surface energy balance
 3. `surf_couple_implicit`: evaluates the land surface energy balance, and evolves the land state (temperatures, soil moisture, and [CASA's](./casa.md) carbon, nitrogen and phosphorus cycles).
 4. `surf_couple_extra`: passes CABLE's runoff variables to the ACCESS river routing scheme.

 The UM is very(!) complex in the sequencing of calculations - some calculations are carried out multiple times per timestep, others less frequently.  Of note `surf_couple_radiation` is only encountered on *radiation timesteps* (every 4th timestep), `surf_couple_explicit` and `surf_couple_extra` are encountered once per timestep, but `surf_couple_implicit` is encountered twice per timestep.

 In contrast to ACCESS-AM3 (UM-CABLE), ACCESS-rAM3 (UM-JULES) undertakes the evolution of the land state as part of `surf_couple_extras`  

Additional technical and scientific links are envisaged to the atmospheric chemistry component [(UKCA)](./ukca.md) and the ice sheet model in due course. 

## Subheading 2 (likely better in [architecture.md](../infrastructure/Architecture.md))

![Structure 2](../../assets/structure2.jpg "Structure 2")
Legend:
- BLUE = Sourced from GitHub
- GREEN = Collections of inputs on vk83
- ORANGE = This experiment’s inputs (dashed is primary area of editing)
- DARK BLUE = in-cycle executions
- BROWN = Output that is deleted
- GREEN = Output that is retained

Notes:  ACCESS-AM3 is built by pulling source code from the ACCESS-NRI UM, [JULES](https://github.com/ACCESS-NRI/JULES), and [CABLE](https://github.com/CABLE-LSM/CABLE) then compiling into a single executuble.

## Key References

 - [Kowalczyk et al. (2006)](https://research.csiro.au/ccam/wp-content/uploads/sites/520/2024/01/1377337424.pdf)