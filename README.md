# Robot Imitation Learning: From Teleoperation to Autonomous Policy
# 機器人模仿學習實戰：從遠端操作到自主策略執行

> **結合工研院實務經驗與交大機器人學習工作坊的實作框架。**
> A practical framework combining industrial experience from ITRI and intensive training at NYCU.

---

## 🏗️ 專案架構 (Repository Structure)

- `📂 assets/`: 存放展示影片、訓練曲線以及實體機器人操作照片。
- `📂 configs/`: 存放訓練參數與模型結構設定 (`.yaml`)，落實參數分離管理。
- `📂 data/`: 定義機器人運動軌跡的數據格式 (如 JSON/HDF5)。
- `📂 scripts/`: 包含遠端操作介面、數據預處理以及模仿學習訓練核心代碼。

---

## 🛠️ 技術背景與實務經驗 (Technical Background)

### 1. 工研院 (ITRI) 實體機器人經驗
在進入此實作專案前，我曾在 **ITRI (工研院)** 參與協作型機器人的數據採集工作。
* **使用設備：** Techman (TM5) 協作機器人。
* **實作內容：** 針對物體抓取 (Pick-and-place) 任務進行軌跡紀錄，並處理視覺感測器與機器人末端夾爪的座標同步。
* **技術心得：** 透過此實務經驗，掌握了實體機器人在執行模仿學習前，高品質專家數據採集 (Expert Demonstration) 的標準流程。

<p align="center">
  <img src="https://github.com/user-attachments/assets/e47479cc-deab-4622-a228-07b76f22e809" width="500" alt="ITRI Techman Robot Data Acquisition">
  <br>
  <i>圖一：於工研院進行 Techman 機器人針對透明目標物的精準夾取數據採集</i>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/d534302a-6376-4238-a5e9-9566d41fb696" width="600" alt="URDF Mimic Joint Demo">
  <br>
  <i>圖二：TM5S-900 與 Robotiq 2F-85 運動學同步展示 (Mimic Joint Verification)</i>
</p>








### 2. 交大機器人學習工作坊 (NYCU Workshop 2026)
本專案的核心目標是在 **交大 329 教室** 的實作課程中，完成一套完整的模仿學習流程：
* **遠端操作 (Teleoperation):** 建立人機介面，採集專家示範數據。
- **模仿學習 (Imitation Learning):** 利用行為複製 (Behavior Cloning) 演算法訓練神經網路。
- **自主執行 (Autonomous Execution):** 在虛擬模擬環境中驗證訓練結果，縮短 Sim-to-Real 的落差。

---

## ⚙️ 技術細節 (Technical Specs)

- **核心演算法:** Behavior Cloning (BC)
- **開發語言:** Python, C# (Unity 串接預留)
- **關鍵工具:** PyTorch, ROS2, Unity / Isaac Sim
- **數據格式:** 結構化軌跡數據，便於未來與 **LLM 高階規劃 (High-level planning)** 整合。


---

## 🔧 技術深研：機器人建模與運動學 (Robot Modeling & Kinematics)

在整合 **Techman TM5S-900** 與 **Robotiq 2F-85** 的過程中，我針對複雜的平行連桿機構進行了優化處理：

### 1. 閉環連桿問題解決 (Solving Closed-loop Kinematics)
* **技術挑戰：** 由於 URDF 原生僅支援樹狀拓樸 (Tree Topology)，無法直接定義 Robotiq 夾爪的閉環平行連桿，導致模擬時會出現末端脫離（脫臼）現象。
* **解決方案：** * **Mimic Joints:** 透過實作 `<mimic>` 標籤（如下方代碼），讓多個連桿節點同步主動關節的位移。
    * **Physics Constraints:** 在模擬器層級（如 Unity/Isaac Sim）補足物理約束，確保運動學路徑的連續性。

#### **URDF 實作片段 (Mimic Joint Snippet):**
```xml
<joint name="robotiq_85_left_inner_knuckle_joint" type="revolute">
  <parent link="robotiq_85_base_link"/>
  <child link="robotiq_85_left_inner_knuckle_link"/>
  <mimic joint="robotiq_85_left_knuckle_joint" multiplier="1.0" offset="0"/>
</joint>

```
---

## 📈 實作進度 (Milestones)

- [x] **建立專業 Repo 框架 (2026/02/07)**
- [x] **匯入工研院實務背景數據 (2026/02/07)**
- [ ] **Day 1 (02/11):** 完成遠端操作介面與首批數據採集。
- [ ] **Day 2 (02/12):** 完成模型訓練與小組挑戰賽 Demo。

