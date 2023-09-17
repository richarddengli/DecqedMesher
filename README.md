# DecqedMesher.jl

[![Build Status](https://github.com/richarddengli/DecqedMesher/actions/workflows/CI.yml/badge.svg?branch=main)](https://github.com/richarddengli/DecqedMesher/actions/workflows/CI.yml?query=branch%3Amain)
[![Coverage](https://codecov.io/gh/richarddengli/DecqedMesher/branch/main/graph/badge.svg)](https://codecov.io/gh/richarddengli/DecqedMesher)

``DecqedMesher.jl`` is a Julia package for constructing primal and dual mesh objects present in the discrete exterior calculus formulation of quantum electrodynamics (DEC-QED). Eventually, ``DecqedMesher.jl`` will be integrated fully into the larger [``DEC-QED computational toolbox``](https://github.com/dnpham23/DEC-QED). Both ``DecqedMesher.jl`` and ``DEC-QED computational toolbox`` are under active development.

A detailed presentation of DEC-QED, its applications for modeling electromagnetic systems, and some results using the mesher within the computational toolbox are contained in the following references:
- [Flux-based three-dimensional electrodynamic modeling approach to superconducting circuits and materials](https://journals.aps.org/pra/abstract/10.1103/PhysRevA.107.053704)
- [Spectral Theory for Non-linear Superconducting Microwave Systems: Extracting Relaxation Rates and Mode Hybridization](https://arxiv.org/abs/2309.03435)

# Installation
`DecqedMesher.jl` is not currently registered in the official [Julia package registry](https://github.com/JuliaRegistries/General), but installation is simple and follows the directions listed [here](https://pkgdocs.julialang.org/v1/environments/#Using-someone-else's-project).

In the command prompt, navigate to the desired directory (via `cd`), and then clone the package via:
```
(shell) git clone https://github.com/richarddengli/DecqedMesher.jl.git
```

Start the Julia REPL and then enter into Pkg REPL (by typing `]`). Then call:
```
(@v1.9) pkg> activate DecqedMesher.jl
```

and also call:
```
(DecqedMesher) pkg> instantiate
```
to activate the package and prepare the project environment.

# Usage
## Making the primal and dual mesh
The 2 user-facing functions in `DecqedMesher.jl` are `complete_dualmesh()` and `complete_dualmesh_2D()`, which take in as input a `.msh` file representing 3D and 2D primal mesh, respectively, and return a length 4 tuple of structs containing the following information corresponding to the `.msh` file:
1. dual mesh information
2. primal mesh information (parsed from  `.msh` and updated with certain information)
3. physical group names (parsed unchanged from  `.msh`)
4. elementary entities (parsed unchaged from  `.msh`)

In a `.jl` file, import `DecqedMesher.jl`:
```julia
using DecqedMesher
```

To construct the dual mesh of a 3D mesh, use `complete_dualmesh("[/path/to/mesh]")`. For example, for a mesh file named `3D_testmesh.msh` in the same directory as the `.jl` file, use:
```julia
dualmesh_3D, primalmesh_3D, physicalnames_dict_3D, all_entities_struct_3D = complete_dualmesh("/3D_testmesh.msh")
```

Similarly, to construct the dual mesh of a 2D mesh, use `complete_dualmesh_2D("[/path/to/mesh]")`. For example, for a mesh file named `2D_testmesh.msh`, use:
```julia
dualmesh_2D, primalmesh_2D, physicalnames_dict_2D, all_entities_struct_2D = complete_dualmesh("/2D_testmesh.msh")
```

All other functions are not intended to be user-facing, and thus must be executed via a qualified call; for example, `DecqedMesher.Mesher3D_Dualmesh.get_supportvolume`.

## Accessing mesh information
Successively access struct fields or index into dictionaries to retrieve the desired information. There are some examples:
```julia
# 3D mesh

# For the 3D mesh, get the coordinates of the primal node whose id is 1
primalmesh_3D.nodedict[1].coords

# For the 2D mesh, get the boundary dual edges beloging to the dual face whose id is 5 (i.e. it is dual to the primal node whose id is 5)
dualmesh_2D.dualfacedict_2D[5].boundary_dualedges
```

The exact sequence of calls for any desired information can be determined by inspecting the hierarchy of data types defined in `mesher3D_types.jl` and `mesher2D_types.jl`. Please note that there are slight differences in somes definitions between the two files. For instance, `mesher2D_types.jl` defines some fields with the additional suffix `_2D` compared to the 3D definitions, as shown by the above example.

# Authors and Acknowledgements
Richard Li, Dzung Pham, Nick Bronn, Thomas McConkey, Olivia Lanes, Hakan Türeci

The development for ``DecqedMesher`` started during the 2022 Undergraduate Research Internship at IBM and Princeton (QURIP), as part of a collaboration between the following groups:
- Türeci Group, Department of Electrical & Computer Engineering, Princeton University
- Qiskit Community Team, IBM Quantum

We also thank Abeer Vaishnav, Nathalie de Leon, Hwajung Kang, and the rest of the 2022 QURIP cohort for their support.
