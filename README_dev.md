# 📘 ScanGeoMeasure — Backend PoC Development Guide

このドキュメントは、ScanGeoMeasure の PoC 開発を進める際に **AI（ChatGPT / Cursor）・開発者自身** が参照するためのガイドです。  
2-1 〜 2-8 の処理ステップを **1ステップ＝1ファイル** で分離し、段階的にテストしながら開発する方針です。

---

## 1. ディレクトリ構成（Backend）

```
backend/
  step2_1_pdf_loader.py
  step2_2_color_extraction.py
  step2_3_denoise.py
  step2_4_skeletonize.py
  step2_5_polylines.py
  step2_6_nodes.py
  step2_7_intersections.py
  step2_8_length.py
  README_dev.md
frontend/
README.md
```

### 📌 分割方針
- **ステップごとにテスト可能**
- 各ステップは `__main__` で単体テスト可能
- 後のステップでは前のステップの関数を import
- 依存関係が明確で、バグ調査が容易

---

## 2. 開発ステップ（2-1〜2-8）とテスト手順

---

### ✅ STEP 2-1: PDF → PNG 変換

```bash
python backend/step2_1_pdf_loader.py your.pdf
```

出力：`debug_page.png`

---

### ✅ STEP 2-2: 色抽出（HSV, 赤/青）

```bash
python backend/step2_2_color_extraction.py your.pdf
```

---

### ✅ STEP 2-3: ノイズ除去（モルフォロジ）

```bash
python backend/step2_3_denoise.py your.pdf
```

---

### ✅ STEP 2-4: Skeletonization（細線化）

```bash
python backend/step2_4_skeletonize.py your.pdf
```

---

### ✅ STEP 2-5: ポリライン抽出（BFS/DFS）

```bash
python backend/step2_5_polylines.py your.pdf
```

---

### ✅ STEP 2-6: 節点抽出

```bash
python backend/step2_6_nodes.py your.pdf
```

---

### ✅ STEP 2-7: 交点抽出（degree ≥ 3）

```bash
python backend/step2_7_intersections.py your.pdf
```

---

### ✅ STEP 2-8: 長さ計算（px → mm → m）

```bash
python backend/step2_8_length.py your.pdf 0.5
```

---

## 3. 開発ルール

### 📌 1. ステップ単位でテスト  
### 📌 2. 副作用を入れない  
### 📌 3. debug PNG を必ず保存  
### 📌 4. AI への指示テンプレを守る  

---

## 4. 使用ライブラリ
- pdf2image  
- opencv-python  
- numpy  
- scikit-image  

---

## 5. 今後の追加予定（Step3〜4）
- FastAPI  
- React + Konva  
- スケール設定  
- CSV 出力  

---

## 6. PoC の完成形
- PDF 読み込み  
- 赤線 & 青線抽出  
- スケルトン化  
- ポリライン抽出  
- 節点 / 交点  
- 延長(m)計算  
- CSV ダウンロード  

