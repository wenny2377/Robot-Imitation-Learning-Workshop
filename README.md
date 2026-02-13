2026/01/19-23 ITRI
2026/02/11-12 NYCU

https://github.com/HCIS-Lab/physical-ai-umi/tree/dev

<img width="1536" height="614" alt="image" src="https://github.com/user-attachments/assets/04527829-e044-4015-b2af-d530f3fa63ed" />

https://github.com/user-attachments/assets/12332e5b-3822-4dbc-8a3d-d747a40e87df


https://github.com/user-attachments/assets/96dbcf79-f9cf-4fc2-a633-fb94559309fb



https://github.com/user-attachments/assets/a494ec30-5ae8-48f1-9711-12e8d1b825fb




https://github.com/user-attachments/assets/74aa47d2-6628-44e2-984e-266b650fd6c2







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


https://github.com/user-attachments/assets/4690878b-054e-4f42-8d15-6633e2d1879b



## 🏗️ 具身智能實戰：UMI x Isaac Sim 流程

本專案利用國立陽明交通大學 HCIS Lab 提供之預建環境 ，實作從現實採集到模擬訓練的完整 Physical AI 閉環。

### 1. 數據採集標準 (Data Collection)

為了確保模型能正確學習動作，數據採集須嚴格遵守以下規格 ：

**影像規格**：GoPro 須設定為 2.7K 120fps、4:3 魚眼視角 。

**標準流程**：遵循  順序 。
 
**環境控管**：校準時需移除多餘標籤（Tags），並在完成後以黑布覆蓋主要校準碼（Tag #13）以避免干擾 。

**採集策略**：物體須置於鏡頭中心，初始影格須完整捕捉 ArUco tags，且相機需接近鳥瞰視角（BEV） 。



### 2. UMI 數據管線 (Pipeline)

透過 `umi-pipeline` 處理原始影片，生成機器人運動軌跡數據 ：
 
**關鍵步驟**：包含提取 IMU 數據、建立 SLAM 地圖、偵測 ArUco 標籤及計算物體位姿（Object Pose） 。

**視覺化檢查**：利用 Jupyter Lab 的 `dataset_visualizer.ipynb` 審核示範品質，檢查是否發生影格丟失或軌跡異常 。



---

## 💡 實作反思與架構探討 (Deep Reflections)

在完成 UMI x Isaac Sim 的全流程實作後，我針對**具身智能 (Embodied AI)** 的未來實作方向整理了兩點核心反思：

### 1. 運動軌跡的層級化整合 (Hierarchical Approach & Motion Planning)

在本次實作中，我們傾向於將端到端（End-to-End）的運動軌跡全部交給模型訓練，但在實際應用中，這種做法效率不一定最高。

* 
**層級式強化學習 (Hierarchical RL)**：應將任務拆解，高層級負責邏輯決策（如：走到桌邊），低層級負責精細操作 。


* **混合策略 (Hybrid Framework)**：
* **前段導航/接近**：可以使用成熟的 **Motion Planning**（如 MoveIt 或導航演算法）負責大範圍、無障礙的位移，這部分不需要耗費大量數據訓練。
* **後段精細操作**：僅針對「最難的部分」（例如：精準抓取目標物、避開脆弱障礙物）進行 **Imitation Learning / RL** 訓練。


* **優點**：這種「Planning + Training」的結合能大幅降低訓練數據量需求，並提高系統在陌生環境的泛化能力。

### 2. 多維度的評估標準與安全性 (Evaluation Metrics & Safety)

目前的實驗多以「成功率」作為唯一指標，但作為**居家輔具機器人 (Assistive Robotics)**，評估標準必須更加多元：

* **安全性 (Safety First)**：這是我論文核心關注點。例如在執行「挖取東西」或「遞送物品」時，模型不僅要達成目標，更要監控末端執行器的力道與加速度，避免碰撞使用者。
* **穩定性指標**：除了成功次數，還需評估軌跡的**平滑度 (Smoothness)**。過度抖動的動作在現實中會造成硬體損耗，且會給予使用者（特別是長者）不安感。
* 
**失效模式分析 (Failure Mode Analysis)**：紀錄 88% 的錯誤來源（如數據品質 ）並分類，是為了建立更強健的自動化評估系統，而不僅僅是追求模型收斂。




