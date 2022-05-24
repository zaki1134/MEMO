# Memo

## Setup

|                     | General                            |
| ------------------- | ---------------------------------- |
| setting file export | `/file/write-setting ["filename"]` |
| setting file read   | `/file/read-setting ["filename"]`  |
| mesh_scale          | `/mesh/scale 0.001 0.001 0.001`    |
| unit                | `/define/units Length mm`          |

|         | Model                                            |
| ------- | ------------------------------------------------ |
| solver  | `/define/models/solver pressure-based y`         |
| time    | `/define/models/steady? y`                       |
|         | `/define/models/unsteady-1st-order? y`           |
| viscous | `/define/models/viscous kw-sst? y`               |
|         | `/define/models/viscous kw-low-re-correction? y` |
| energy  | `/define/models/energy? y n n n y`               |
|         | Enable energy model?                             |
|         | Compute viscous energy dissipation?              |
|         | include pressure work in energy equation?        |
|         | include kinetic energy in energy equation?       |
|         | include diffusion as inlets?                     |
| species | `/define/models/species/species-transport? y`    |
|         | `mixture-template`                               |
|         |                                                  |

|         | Material                                           |
| ------- | -------------------------------------------------- |
| fluid   | `/define/materials/copy fluid nitrogen`            |
|         | `/define/materials/copy fluid carbon-dioxide`      |
| mixture | `/define/materials/copy mixture mixture-template`  |
|         | `/define/materials/change-create mixtute-template` |

|     | CellZone |
| --- | -------- |
|     | `***`    |

|     | BoundaryCondition |
| --- | ----------------- |
|     | `***`             |

|      | NamedExpressions                                              |
| ---- | ------------------------------------------------------------- |
| add  | `/define/named-expressions/add [xxx] definition "aaa [K]" .`  |
| edit | `/define/named-expressions/edit [xxx] definition "bbb [K]" .` |

</br>
</br>

## Solution

|     | ReportDefinitions |
| --- | ----------------- |
|     | `****`            |

</br>
</br>

## Results

|     | Reports                                                                            |
| --- | ---------------------------------------------------------------------------------- |
| AWA | `/report/surface-integrals/area-weighted-avg [int_* inlet*] () [var] y [filename]` |
