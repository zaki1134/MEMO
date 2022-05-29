# 2022R1

## Setup

|            | General                         |
| ---------- | ------------------------------- |
| mesh_scale | `/mesh/scale 0.001 0.001 0.001` |
| unit       | `/define/units Length mm`       |

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

|                            | Monitors                                                                     |
| -------------------------- | ---------------------------------------------------------------------------- |
| Residual Check-Convergence | `/solve/monitors/residual/check-convergence? n n n n n n n n n n`            |
| Convergence Conditions     | `/solve/convergence-conditions/conv-reports/add [ConvergenceConditionsName]` |
|                            | `report-defs [ReposrtDefinitionsName]`                                       |
|                            | `stop-criterion 1e-3`                                                        |
|                            | `initial-values-to-ignore 500`                                               |
|                            | `previous-values-to-consider 500`                                            |
|                            | `print? y`                                                                   |

|     | ReportDefinitions                                                                   |
| --- | ----------------------------------------------------------------------------------- |
| add | `/solve/report-definitions/add [ReportDefinitionName] [RD(report-definition-type)]` |
|     | `field [RD(field)]`                                                                 |
|     | `per-surface? n`                                                                    |
|     | `report-type [RD(report-type)]`                                                     |
|     | `surface-names [SurfaceName]`                                                       |

|     | ExecuteCommands                                                                          |
| --- | ---------------------------------------------------------------------------------------- |
| add | `/solve/execute-commands/add-edit [ExecuteCommandsName] 300 iteration "ExecuteCommands"` |

</br>
</br>

## Results

|        | Reports                                                                                         |
| ------ | ----------------------------------------------------------------------------------------------- |
| export | `/report/surface-integrals/[Reports(report-type)] [SurfaceName] () [Variable] y [FileName.srp]` |

</br>
</br>

## import & export

|                     | TUI Command                                                         |
| ------------------- | ------------------------------------------------------------------- |
| setting-file export | `/file/write-setting [FileName.txt]`                                |
| setting-file import | `/file/read-setting [FileName.txt]`                                 |
| CFD-Post            | `/file/export-to-cfd-post [FileName.cdat] () * () [Variable] . y n` |
| Ensight             | `/file/export/ensight-gold [FileName] [Variable] . y * () () n`     |
