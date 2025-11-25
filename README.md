# Install-Tensorflow-on-linux
TensorFlow 2.15 + Python 3.11 Environment Setup with Albumentations package.

---

# TensorFlow 2.15 + Python 3.11 Environment Setup (建立 TensorFlow 2.15 + Python 3.11 環境)

## 1. 建立新的 Python 3.11 環境

```bash
conda create -n tf215_py311 python=3.11 -y
conda activate tf215_py311
pip install --upgrade pip
```

---

## 2. 安裝帶 GPU 的 TensorFlow

```bash
pip install "tensorflow[and-cuda]==2.15.*"
```

---

## 3. 安裝 Albumentations（注意不要動到 numpy 版本，保持 1.26.4）

```bash
pip install "numpy==1.26.4"
pip install "opencv-python-headless==4.8.1.78"
pip install "albumentations==1.3.1" "qudida==0.0.4"
```

---

## 4. 安裝你之後需要的其他套件

```bash
pip install <package-name>
```

---
