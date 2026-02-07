# Robot Description Files

本資料夾存放整合後的機器人描述檔，用於模擬環境與運動學計算。
This folder contains integrated robot description files for simulation and kinematics.

## 🤖 機器人配置 (Robot Configuration)
- **Base Arm:** Techman TM5S-900 (900mm reach, 5kg payload)
- **End-Effector:** Robotiq 2F-85 Adaptive Gripper
- **Status:** Fully integrated via URDF.

## 🛠️ 技術實作 (Technical Implementation)
- **Mimic Joint Constraint:** 解決了 Robotiq 2F-85 在 URDF 中因閉環連桿導致的脫臼問題，透過 `<mimic>` 標籤實現內外連桿同步。
- **Mesh Optimization:** 使用 `.dae` (Visual) 與 `.stl` (Collision) 分離管理，提升模擬效能。

### 檔案清單：
- `tm5s_robotiq_85.urdf`: 核心整合描述檔。
- `meshes/`: (預留) 存放 3D 模型檔案路徑。