# DMN Online Prediction Solver

This tool is designed for efficient non-linear Finite Element Analysis (FEA) using **Deep Material Networks (DMN)**. It enables users to execute complex multi-phase simulations by configuring YAML input files without requiring access to the Python source code.

---

### 1. Execution via Command Line
The solver is provided as a compiled executable (`DMN_Solver.exe`). You can run it via PowerShell or Command Prompt using the following arguments:

| Argument | Type | Default Value | Description |
| :--- | :--- | :--- | :--- |
| `--geometry_path` | `Path` | `.\DNS\2d\n5\geometry.yaml` | Path to the mesh geometry and nodal coordinates. |
| `--material_path` | `Path` | `.\DNS\2d\n5\material_DMN.yaml` | Path to DMN weights. **Phase 1**: Matrix (Elasto-Plasticity). **Phase 2**: Fiber (Elasticity). |
| `--loading_path` | `Path` | `.\DNS\2d\n5\loading.yaml` | Path to boundary conditions and the non-linear loading sequence. |
| `--nLoad` | `int` | `100` | Total number of increments/steps for the simulation. |
| `--nPrint` | `int` | `1` | Frequency of saving output (e.g., save results every N steps). |
| `--device` | `str` | `cpu` | Computational engine: `cpu` or `cuda`. |

**Example Command:**
```powershell
.\DMN_Solver.exe --nLoad 100 --loading_path ".\DNS\2d\demo\loading.yaml" --material_path ".\DNS\2d\demo\material_DMN.yaml" --geometry_path ".\DNS\2d\demo\geometry.yaml"
```

---

### 2. Execution via Command Line
The solver is provided as a compiled executable (`DMN_Solver.exe`). You can run it via PowerShell or Command Prompt by passing the following arguments:

DMN_Solver can be downloaded from https://drive.google.com/drive/folders/1s_tC3h3VD2n3XXccqObD5xx7lwWsWsrN?usp=sharing

```text
DMN-Online-Prediction-Solver/
├── DMN_Solver/DMN_Solver.exe # Main Executable
├── README.md                 # Instructions
└── DNS/                      # Input Data Folder
    └── 2d/
        └── demo/
            ├── loading.yaml                                  # Edit this to change loading paths
            ├── geometry.yaml                                 # Nodal coordinates
            ├── material_DMN.yaml                             # DMN Phase properties
            ├── Binary2_5_lr_0.1_batchsize_20_20241115_204415 # Trained DMN paramters
            └── mesh.k                                        # Mesh file (Viewable in LS-PREPOST)
```

---

### 3. Input Configuration (`loading.yaml`)
The most common user interaction is defining a custom loading history. You can edit the loading_points in `loading.yaml`. The solver automatically performs linear interpolation between these points based on the `--nLoad` count.

Boundary:
  # Format: [Progress_Ratio (0.0 to 1.0), Load_Factor]
  loading_points:
    - [0.0, 0.0]  # Initial state
    - [0.3, 0.5]  # Peak 1: Reach 0.5 factor at 30% of simulation
    - [0.6, 0.2]  # Unload: Drop to 0.2 factor at 60% of simulation
    - [1.0, 1.0]  # Final: Reach 1.0 factor at the end

