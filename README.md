# 🤖 統計迴歸分析：多模態 RAG AI 助教系統
**(Multimodal RAG AI Teaching Assistant for Statistical Regression Analysis)**

本專案旨在解決大學統計學/迴歸分析課程中，全英文教材晦澀難懂、傳統大語言模型 (LLM) 難以精準解析統計數學公式的痛點。透過開發「**多模態檢索增強生成系統 (Multimodal RAG)**」，結合視覺模型與本地端向量資料庫，打造出能讀懂 PDF、圖片及 LaTeX 數學公式的專屬 AI 助教。

## 🛠️ 核心技術棧 (Tech Stack)
* **大型語言模型 (LLM) & 視覺模型 (Vision)**: `Grok-3-mini-beta`, `Grok-vision-beta`
* **向量化模型 (Embedding Model)**: `sentence-transformers` (`all-MiniLM-L6-v2`)
* **向量資料庫 (Vector Database)**: `FAISS` (Facebook AI Similarity Search)
* **文件解析與光學辨識 (PDF/OCR)**: `pdf2image`, `base64`
* **前端互動介面 (UI)**: `Gradio`

## ✨ 系統特色與解決方案 (Key Features)

### 1. 多模態輸入支援 (Multimodal Input)
傳統 RAG 僅支援純文字檢索，本系統整合 `Grok-vision-beta` API，允許使用者直接上傳講義截圖或 PDF 檔案。系統會自動將圖像轉換為 Base64 編碼並進行高精度 OCR 內容提取。

### 2. LaTeX 數學公式精準渲染 (LaTeX Math Rendering)
統計學講義包含大量複雜公式。本專案透過兩階段處理確保公式的正確性：
* **Prompt Engineering**: 強制要求 Vision 模型提取文字時保留原始 LaTeX 語法。
* **字串預處理**: 透過 Python 腳本將 `\[ ... \]` 標籤動態替換為 `$$...$$`，確保 Gradio 前端 Markdown 引擎能完美渲染數學公式，大幅提升閱讀體驗。

### 3. 本地端高效檢索 (Local Vector Search)
預先將迴歸分析教材切塊 (Chunking) 並轉換為向量，儲存於 `FAISS` 建立的 L2 距離索引庫中。當使用者提問時，系統會計算 Cosine Similarity，精準抓取 Top-3 最相關的講義片段作為上下文 (Context) 餵給生成模型，有效降低 LLM 的幻覺 (Hallucination)。

## 📸 系統畫面展示 (Demo)
<img width="801" height="339" alt="image" src="https://github.com/user-attachments/assets/59fb65da-4dc2-4a39-870e-d0f6c9f6d0ad" />


## 🚀 執行與使用方式 (How to Run)
強烈建議使用 **Google Colab** 執行以獲得最佳環境相容性：
1. 取得本專案的 Jupyter Notebook 檔。
2. 準備您的 `Grok API Key`，並將其設定於 Colab 的 Secrets 中（命名為 `Grok`）。
3. 依序執行儲存格，程式將自動從雲端下載已建立好的講義 FAISS 向量庫 (`lecture_embeddings.zip`)。
4. 運行結束後將生成 Gradio 網頁連結，即可開始上傳圖片或輸入文字與 AI 助教進行互動。

## 🎯 專案動機 (Motivation)
本專案源於個人學習需求。面對全英授課與難以消化的大量公式， ChatGPT 4o模型 往往在解答專業統計問題時產生幻覺，或無法讀懂截圖中的公式。因此決定結合資料科學導論所學，親手刻出這個專屬的 RAG 系統，作為自我重修與學習的最佳輔助工具。
