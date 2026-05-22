# CABLE

This section is a work in progress!

The [CABLE land model](https://github.com/CABLE-LSM/CABLE) forms the land component of the ACCESS family of climate and Earth System models.
There are two major subcomponents: CABLE which provides the biophysical information ACCESS requires each time step, and CASA which provides the terrestrial carbon cycle information for ACCESS-ESM.
CABLE is carefully linked to the atmospheric model (UM) via the native land model of the UM [(JULES)](https://github.com/ACCESS-NRI/JULES).
Additional links are envisaged to the atmospheric chemistry component (UKCA) and ice sheet model in due course.  


## Subheading 1

![Structure 1](../../assets/structure1.jpg "Structure 1")

Legend:
- BLUE is UM code
- BEIGE is JULES code
- BROWN is JULES-CABLE coupling code (no science)
- GREEN is CABLE science code and JULES-CABLE coupling code (where necessary)
- DIAGONAL LINES indicate links to output via STASH

## Subheading 2 (likely better in the architecture.md)

![Structure 2](../../assets/structure2.jpg "Structure 2")

Legend:
- BLUE = Sourced from GitHub
- GREEN = Collections of inputs on vk83
- ORANGE = This experiment’s inputs (dashed is primary area of editing)
- DARK BLUE = in-cycle executions
- BROWN = Output that is deleted
- GREEN = Output that is retained

## Key References

 - [Kowalczyk et al. (2006)](https://research.csiro.au/ccam/wp-content/uploads/sites/520/2024/01/1377337424.pdf)