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
| species | `****`                                           |

|         | Material                                           |
| ------- | -------------------------------------------------- |
| fluid   | `/define/materials/copy fluid nitrogen`            |
|         | `/define/materials/copy fluid carbon-dioxide`      |
| mixture | `/define/materials/copy mixture mixture-template`  |
|         | `/define/materials/change-create mixtute-template` |

|     | CellZone |
| --- | -------- |
|     | `****`   |

|     | BoundaryCondition |
| --- | ----------------- |
|     | `****`            |

|      | NamedExpressions                                              |
| ---- | ------------------------------------------------------------- |
| add  | `/define/named-expressions/add [xxx] definition "aaa [K]" .`  |
| edit | `/define/named-expressions/edit [xxx] definition "bbb [K]" .` |

</br>
</br>

## Solution

|     | Methods |
| --- | ------- |
|     | `****`  |

|     | Controls |
| --- | -------- |
|     | `****`   |

|     | Report Definitions                                                                  |
| --- | ----------------------------------------------------------------------------------- |
| add | `/solve/report-definitions/add [ReportDefinitionName] [RD(report-definition-type)]` |
|     | `field [RD(field)]`                                                                 |
|     | `per-surface? n`                                                                    |
|     | `report-type [RD(report-type)]`                                                     |
|     | `surface-names [SurfaceName]`                                                       |

|                       | Monitors                                                                     |
| --------------------- | ---------------------------------------------------------------------------- |
| Residual              | `/solve/monitors/residual/check-convergence? n n n n n n n n n n`            |
|                       | 1 continuity residuals                                                       |
|                       | 2 x-velocity residuals                                                       |
|                       | 3 y-velocity residuals                                                       |
|                       | 4 z-velocity residuals                                                       |
|                       | 5 energy residuals                                                           |
|                       | 6 k residuals                                                                |
|                       | 7 omega residuals                                                            |
|                       | 8 species-0                                                                  |
|                       | 9 species-1...                                                               |
| ConvergenceConditions | `/solve/convergence-conditions/conv-reports/add [ConvergenceConditionsName]` |
|                       | `report-defs [ReposrtDefinitionsName]`                                       |
|                       | `stop-criterion 1e-3`                                                        |
|                       | `initial-values-to-ignore 500`                                               |
|                       | `previous-values-to-consider 500`                                            |
|                       | `print? y`                                                                   |

|                      | Initialization                             |
| -------------------- | ------------------------------------------ |
| InitializationMethod | `/solve/initialize/[InitializationMethod]` |
|                      | initialize-flow                            |
|                      | hyb-initialization                         |
|                      | fmg-initialization                         |
| InitialValues        | `/solve/initialize/set-defaults`           |
|                      | `x-velocity 0.0`                           |
|                      | `y-velocity 0.0`                           |
|                      | `z-velocity 0.0`                           |
|                      | `pressure 0.0`                             |
|                      | `k 0.0`                                    |
|                      | `omega 0.0`                                |
|                      | `temperature 0.0`                          |
|                      | `species-0 0.0`                            |
|                      | `species-1 0.0`                            |

|         | Execute Commands                                                                         |
| ------- | ---------------------------------------------------------------------------------------- |
| add     | `/solve/execute-commands/add-edit [ExecuteCommandsName] 300 iteration "ExecuteCommands"` |
| disable | `/solve/execute-commands/disable [ExecuteCommandsName]`                                  |
| enable  | `/solve/execute-commands/enable [ExecuteCommandsName]`                                   |

|                   | Run Caluclation                                                    |
| ----------------- | ------------------------------------------------------------------ |
| PseudoTimeSetting | `/solve/set/pseudo-time-method/global-time-step-settings y 1 0.01` |
|                   | activate automatic time step size calculation?                     |
|                   | length scale calculation methods options:                          |
|                   | 0 - aggressive                                                     |
|                   | 1 - conservative                                                   |
|                   | 2 - user-specified                                                 |
|                   | length scale calculation method                                    |
|                   | auto time step size scaling factor                                 |
|                   | `/solve/set/pseudo-time-method/verbosity 0`                        |
|                   | Set verbosity level: (0 1 2)                                       |

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
