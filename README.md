# kg-diffusion-garment

> 以知識圖譜驅動語意對齊的擴散模型服裝圖像生成框架

## 論文資訊

**標題：** Diffusion-Based Garment Synthesis via Knowledge Graph-Driven Structural Cross-Modal Semantic Alignment

**作者：** Miao-Yin Chen, Shu-Han Chuang

**單位：** Department of Data Science, Soochow University, Taipei, Taiwan

**發表：** 2025 IEEE Artificial Intelligence and Smart Technology Applications Symposium (IEEE AISTA 2025), July 15–17, 2025

**關鍵詞：** Diffusion-Based Garment Synthesis, Knowledge Graph, OpenPose, Cross-Modal Semantic Alignment

---

## 研究簡介

本研究提出一個結合知識圖譜語意推理與擴散模型生成能力的服裝圖像合成框架，旨在解決現有文字驅動圖像生成方法中常見的兩大核心挑戰：

- **屬性混淆（Attribute Confusion）**：顏色、材質、花紋等視覺屬性被錯誤地對應到服裝的不同部位
- **區域不一致（Regional Inconsistency）**：生成過程中對無關區域造成非預期的修改，破壞整體視覺結構

---

## 方法架構

生成流程分為四個主要階段：

1. **語意文字編碼**：透過 Text2Graph 模組，將自然語言描述轉換為結構化語意三元組（主體、關係、屬性），例如：
   - `(jacket, has-color, navy blue)`
   - `(collar, has-style, straight-point)`

2. **潛在空間建構**：使用 CLIP 視覺語言編碼器將三元組嵌入共享潛在空間，作為生成過程的語意引導信號

3. **迭代去噪生成**：以 Stable Diffusion v1.4 為基礎模型，透過 LoRA 進行參數高效微調，降低訓練成本並提升時尚領域的適應性

4. **結構條件控制**：整合 ControlNet 以姿勢骨架（OpenPose）為輔助輸入，強化空間布局一致性

---

## 資料集

使用 Kaggle 上的 [H&M Personalized Fashion Recommendations](https://www.kaggle.com/competitions/h-and-m-personalized-fashion-recommendations) 資料集子集：

- 約 **105,000** 件時尚商品
- 每件商品包含高解析度正面商品圖與詳細文字描述
- 訓練圖像統一縮放至 **512 × 512** 像素

---

## 訓練設定

| 參數 | 設定值 |
|------|--------|
| 基礎模型 | Stable Diffusion v1.4 |
| 微調方法 | LoRA |
| 訓練迭代次數 | 200,000 |
| 優化器 | AdamW |
| 學習率 | 1×10⁻⁶ |
| Gradient Accumulation | 4 步（等效 batch size = 1） |
| 硬體 | NVIDIA A6000 GPU |

---

## 實驗結果

與現有方法的定量比較（↓ 越低越好 / ↑ 越高越好）：

| 模型 | CLIP Score ↑ | FID Score ↓ | Inception Score ↑ |
|------|-------------|-------------|-------------------|
| Stable Diffusion v1.4 | 31.54 | 290.15 | 1.76 |
| Fine-tuned Stable Diffusion | 28.56 | 275.35 | 1.89 |
| **KG-driven Stable Diffusion（本研究）** | **29.28** | **255.90** | **1.86** |

本研究在 FID 分數上達到最低值 **25.90**，顯示生成圖像與真實圖像分布最為接近，視覺真實性最優。

---

## 專案結構

```
kg-diffusion-garment/
│
├── README.md                          # 本說明文件
└── Diffusion-Based_Garment_Synthesis  # 論文 PDF
    _via_Knowledge_Graph-Driven_...pdf
```

---

## 引用

如您的研究有參考本論文，請使用以下 BibTeX 格式引用：

```bibtex
@article{chen2024kg-diffusion-garment,
  title     = {Diffusion-Based Garment Synthesis via Knowledge Graph-Driven
               Structural Cross-Modal Semantic Alignment},
  author    = {Miao-Yin Chen and Shu-Han Chuang},
  institution = {Department of Data Science, Soochow University},
  year      = {2024}
}
```

---

## 相關研究

- [DiffCloth](https://arxiv.org/abs/2308.11206) — 本研究的重要參考基準
- [ControlNet](https://arxiv.org/abs/2302.05543) — 結構條件控制模組
- [LoRA](https://arxiv.org/abs/2106.09685) — 參數高效微調方法
- [Stable Diffusion](https://arxiv.org/abs/2112.10752) — 基礎生成模型
