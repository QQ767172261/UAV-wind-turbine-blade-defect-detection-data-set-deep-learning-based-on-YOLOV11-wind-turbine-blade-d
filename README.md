无人机风电叶片缺陷检测数据集，深度学习基于YOLOV11风电叶片缺陷检测系统 

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/9a5e5e1d8b024ff8988c716f52a7048d.png)
3282张，提供yolo和voc两种标注方式
训练集：2743张，验证集：270张，测试集：269张
6类![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b9f968bf1008454fb6b2a28ee5cf65f8.png)

类别名称: 每一类图像数 ，每一类标注数![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/abf822a483da48c495899fd635eaef27.png)

Crack-裂纹: 1144,2163
Erosion-侵蚀: 233,337
Dirt-脏污: 693,762
oil_leakage-漏油: 574,600
PU-tape pu胶带: 329,621
Pin_Hole-孔洞: 303,303
image num: 3282![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c38c553d28254d0bb9bdf6369202c526.png)

模型训练使用yolov11n训练，50个epoch训练结果，map如图所示。![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/6a9387cf318445a0b3496b199a359609.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c8bfeecbbdec4d7682ebff8753c64856.png)

运行界面采用qt，本项目已经训练好模型，配置好环境后可直接使用，运行效果见图像![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/457c9ae83202445c874c92b3e8506915.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a230f89340304c4aaeacfcd0dc206a80.png)
核对完：3282 = 2743 + 270 + 269，总标注框 4786 个，平均每图 1.46 个。表格在前，简易代码在后。

### 表 1 数据集概览
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/fcf67b21628f4efba44448ed052261a4.png)

| 项目 | 内容 |
| --- | --- |
| 数据集名称 | 风电叶片缺陷检测数据集 |
| 图像总数 | 3282 张 |
| 任务类型 | 目标检测（缺陷定位与分类） |
| 类别数 | 6 类 |
| 标注格式 | YOLO + VOC 两种同源标注（未提供 COCO） |
| 总标注框 | 4786 个，平均每图 1.46 个 |
| 数据划分 | train 2743 / val 270 / test 269 |
| 训练基线 | YOLOv11n，50 epochs（权重已预训练） |
| 配套界面 | Qt（PyQt5）桌面推理端，配置环境后直接运行 |
| 适用场景 | 风电叶片巡检、无人机/机器人叶片检测、缺陷记录归档 |

### 表 2 数据划分

| 子集 | 图像数 | 占比 |
| --- | --- | --- |
| 训练集 train | 2743 | 83.6% |
| 验证集 val | 270 | 8.2% |
| 测试集 test | 269 | 8.2% |
| 合计 | 3282 | 100% |

### 表 3 类别分布![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/058f248b66f34ee48ffcd31d33dea7bf.png)


| 序号 | 类名 | 中文 | 含该类图像数 | 标注框数 | 图像覆盖率 | 平均框/图 |
| --- | --- | --- | --- | --- | --- | --- |
| 0 | Crack | 裂纹 | 1144 | 2163 | 34.9% | 1.89 |
| 1 | Erosion | 侵蚀 | 233 | 337 | 7.1% | 1.45 |
| 2 | Dirt | 脏污 | 693 | 762 | 21.1% | 1.10 |
| 3 | oil_leakage | 漏油 | 574 | 600 | 17.5% | 1.05 |
| 4 | PU-tape | PU 胶带 | 329 | 621 | 10.0% | 1.89 |
| 5 | Pin_Hole | 孔洞 | 303 | 303 | 9.2% | 1.00 |
| — | 合计 | — | — | 4786 | — | 1.46（全库） |

### 表 4 交付内容与运行环境

| 交付项 | 说明 |
| --- | --- |
| 数据集 | 3282 张图 + YOLO / VOC 两套标注 + data.yaml |
| 预训练权重 | `best.pt`（YOLOv11n，50 epoch） |
| 推理程序 | Qt 界面，支持单图导入、结果可视化、缺陷列表输出 |
| 依赖 | Python 3.9–3.11、ultralytics、PyQt5、opencv-python |
| 硬件 | 有 GPU 走 CUDA；无 GPU 自动 CPU 推理 |

### 表 5 使用与调优要点

| 关注点 | 现状 | 建议 |
| --- | --- | --- |
| 类别长尾 | Crack 占 34.9% 的图、2163 框，Erosion 仅 233 张 337 框 | 对 Erosion / Pin_Hole 用类别加权或过采样，报告里单列各类 P/R |
| 缺陷尺度差异 | 裂纹细长、脏污成片、胶带与孔洞面积小 | `imgsz` 用 640 起步，裂纹多可试 832；开 `mosaic`+`scale=0.6` |
| 两套格式一致性 | YOLO 与 VOC 必须同源同框 | 换格式后抽 10 张核对框数，防止转换丢框 |
| 评估口径 | 6 类不均衡，mAP 均值会掩盖小类 | 用 test 269 张验收，按类输出 AP 并画混淆矩阵 |

---

## 一、`data.yaml`

```yaml
path: ./datasets/wind_blade
train: images/train
val: images/val
test: images/test

nc: 6
names:
  0: Crack
  1: Erosion
  2: Dirt
  3: oil_leakage
  4: PU-tape
  5: Pin_Hole
```

## 二、训练 `train.py`（YOLOv11n，50 epoch）

```python
from ultralytics import YOLO

if __name__ == "__main__":
    model = YOLO("yolo11n.pt")

    model.train(
        data="data.yaml",
        epochs=50,
        imgsz=640,
        batch=16,                 # 显存不足改 8
        device=0,                 # 无 GPU 写 "cpu"
        patience=15,
        optimizer="auto",
        cos_lr=True,
        # 缺陷尺度跨度大，增强偏几何与尺度
        scale=0.6,
        degrees=10,
        fliplr=0.5,
        mosaic=1.0,
        mixup=0.1,
        project="runs/detect",
        name="wind_blade_yolo11n",
    )

    m = model.val(data="data.yaml", split="val")
    print("mAP50-95:", m.box.map, " mAP50:", m.box.map50)
```

## 三、单图推理 `predict.py`

```python
from ultralytics import YOLO
import cv2

model = YOLO("runs/detect/wind_blade_yolo11n/weights/best.pt")

res = model("blade.jpg", imgsz=640, conf=0.35, iou=0.5)[0]
vis = res.plot(line_width=2)

for i, box in enumerate(res.boxes):
    cls = model.names[int(box.cls[0])]
    conf = float(box.conf[0])
    x1, y1, x2, y2 = map(int, box.xyxy[0].tolist())
    print(f"{i+1}. {cls} conf={conf:.3f} box=({x1},{y1},{x2},{y2})")

cv2.imwrite("result.jpg", vis)
```

## 四、Qt 界面 `app.py`

```python
import sys, os, glob
import cv2
from PyQt5.QtWidgets import (QApplication, QMainWindow, QWidget, QLabel, QPushButton,
                             QVBoxLayout, QHBoxLayout, QTableWidget, QTableWidgetItem,
                             QFileDialog, QComboBox, QHeaderView)
from PyQt5.QtGui import QImage, QPixmap
from PyQt5.QtCore import Qt
from ultralytics import YOLO


class WindBladeApp(QMainWindow):
    def __init__(self, weights="runs/detect/wind_blade_yolo11n/weights/best.pt"):
        super().__init__()
        self.setWindowTitle("风电叶片缺陷检测系统 - YOLOv11n")
        self.resize(1240, 760)
        self.model = YOLO(weights)
        self.cur = None

        central = QWidget()
        self.setCentralWidget(central)
        root = QHBoxLayout(central)

        # 左侧图像 + 结果表
        left = QVBoxLayout()
        self.img_label = QLabel("请导入风电叶片图片")
        self.img_label.setAlignment(Qt.AlignCenter)
        self.img_label.setMinimumSize(760, 520)
        self.img_label.setStyleSheet("border:1px solid #bbb; background:#fafafa;")
        self.table = QTableWidget(0, 5)
        self.table.setHorizontalHeaderLabels(["序号", "缺陷类别", "置信度", "坐标(x1,y1,x2,y2)", "面积"])
        self.table.horizontalHeader().setSectionResizeMode(QHeaderView.Stretch)
        left.addWidget(self.img_label, 4)
        left.addWidget(self.table, 2)

        # 右侧控制
        right = QVBoxLayout()
        self.btn_img = QPushButton("导入图片")
        self.btn_dir = QPushButton("批量文件夹")
        self.btn_save = QPushButton("保存结果")
        self.btn_exit = QPushButton("退出")
        self.cb = QComboBox()
        self.cb.addItems(["全部类别", "Crack", "Erosion", "Dirt",
                          "oil_leakage", "PU-tape", "Pin_Hole"])
        self.info = QLabel("检测信息\n缺陷数量：-\n耗时：-")
        self.info.setWordWrap(True)
        self.info.setStyleSheet("padding:8px; background:#f2f5f7; border-radius:6px;")

        for w in (self.btn_img, self.btn_dir, self.cb, self.btn_save, self.btn_exit, self.info):
            right.addWidget(w)
        right.addStretch(1)
        root.addLayout(left, 3)
        root.addLayout(right, 1)

        self.btn_img.clicked.connect(self.open_image)
        self.btn_dir.clicked.connect(self.open_dir)
        self.btn_save.clicked.connect(self.save)
        self.btn_exit.clicked.connect(self.close)

    def detect(self, path):
        res = self.model(path, imgsz=640, conf=0.35, iou=0.5, verbose=False)[0]
        vis = res.plot(line_width=2)
        self.cur = vis
        self.show_cv(vis)
        self.table.setRowCount(0)

        filt = self.cb.currentText()
        for i, box in enumerate(res.boxes):
            cls = self.model.names[int(box.cls[0])]
            if filt != "全部类别" and cls != filt:
                continue
            x1, y1, x2, y2 = map(int, box.xyxy[0].tolist())
            r = self.table.rowCount()
            self.table.insertRow(r)
            for c, v in enumerate([str(r + 1), cls, f"{float(box.conf[0]):.3f}",
                                   f"({x1},{y1},{x2},{y2})",
                                   f"{(x2 - x1) * (y2 - y1)}"]):
                self.table.setItem(r, c, QTableWidgetItem(v))

        self.info.setText(f"检测信息\n文件：{os.path.basename(path)}\n"
                          f"缺陷数量：{self.table.rowCount()}\n"
                          f"推理耗时：{res.speed['inference']:.1f} ms")

    def open_image(self):
        p, _ = QFileDialog.getOpenFileName(self, "选择图片", "",
                                           "图片 (*.jpg *.jpeg *.png *.bmp)")
        if p:
            self.detect(p)

    def open_dir(self):
        d = QFileDialog.getExistingDirectory(self, "选择文件夹")
        if not d:
            return
        for f in sorted(glob.glob(os.path.join(d, "*.*"))):
            if f.lower().endswith((".jpg", ".jpeg", ".png", ".bmp")):
                self.detect(f)
                QApplication.processEvents()

    def show_cv(self, bgr):
        rgb = cv2.cvtColor(bgr, cv2.COLOR_BGR2RGB)
        h, w, ch = rgb.shape
        img = QImage(rgb.data, w, h, w * ch, QImage.Format_RGB888)
        self.img_label.setPixmap(QPixmap.fromImage(img).scaled(
            self.img_label.size(), Qt.KeepAspectRatio, Qt.SmoothTransformation))

    def save(self):
        if self.cur is None:
            return
        p, _ = QFileDialog.getSaveFileName(self, "保存", "result.jpg", "JPG (*.jpg)")
        if p:
            cv2.imwrite(p, self.cur)


if __name__ == "__main__":
    app = QApplication(sys.argv)
    win = WindBladeApp()
    win.show()
    sys.exit(app.exec_())
```

## 五、环境

```bash
pip install ultralytics pyqt5 opencv-python

python train.py     # 训练 50 epoch
python app.py       # 打开 Qt 界面

# Windows 打包 exe
pip install pyinstaller
pyinstaller -F -w app.py --collect-all ultralytics
```


