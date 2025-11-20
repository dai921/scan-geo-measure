# ScanGeoMeasure – 手書き図面の線長・点数自動抽出システム（PoC）
ScanGeoMeasure は、手書き（色ペン）で描かれた線や点をスキャン PDF から抽出し、色付き線の長さ・節点・交点を自動解析する PoC です。UI で最終確認・補正が可能です。
## 目的
- 色線の長さ（m）を自動算出
- ポリライン数・節点数・交点数の抽出
- クライアントは最小操作で確認と修正が可能
- ラスタ PDF 前提の画像処理方式
## システム概要
### 1. 自動抽出（Python 画像処理）
- PDF → PNG 高解像度変換
- HSV 色抽出
- ノイズ除去
- 細線化（skeletonize）
- ポリライン抽出
- 節点抽出
- 交点抽出
### 2. UI スケール設定
- 既知長（例：6000mm）を 1 本なぞって px→mm 換算
### 3. UI の補正機能
- ON/OFF、色変更、マージ、分割、手描き補正
### 4. CSV 出力
- 色別総延長、ポリライン長、節点数、交点数
## 処理フロー
```python
from pdf2image import convert_from_path
import cv2
import numpy as np
from skimage.morphology import skeletonize
pages = convert_from_path("input.pdf", dpi=400)
img = cv2.cvtColor(np.array(pages[0]), cv2.COLOR_RGB2BGR)
hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
mask = cv2.inRange(hsv, lower, upper)
kernel = np.ones((3,3), np.uint8)
clean = cv2.morphologyEx(mask, cv2.MORPH_OPEN, kernel)
clean = cv2.morphologyEx(clean, cv2.MORPH_CLOSE, kernel)
skeleton = skeletonize(clean > 0).astype(np.uint8)
polylines = extract_polylines(skeleton)
nodes = extract_nodes_from_skeleton(skeleton)
intersections = detect_intersections(skeleton)
length_m = (length_px * scale_mm_per_px) / 1000
```
## 仕様技術
### バックエンド
- Python（FastAPI）
- OpenCV, scikit-image, pdf2image, numpy
### フロントエンド
- React, react-konva, file-saver
## ディレクトリ構成
ScanGeoMeasure/
├── backend/
│   └── processing/
│       ├── pdf_loader.py
│       ├── color_extract.py
│       ├── skeleton.py
│       ├── polyline.py
│       ├── nodes.py
│       ├── intersections.py
│       └── scale.py
└── frontend/
    └── src/
