# TensorFlow GPU Usage Operational Guidelines

This document defines the standard operating procedure (SOP) for verifying GPU availability and configuring TensorFlow GPU memory behavior on Linux systems with NVIDIA GPUs.

---

## 1. Terminal Checks (System-Level Verification)

Use the following commands to verify that the NVIDIA driver, GPU, and CUDA environment are functioning correctly.

### 1.1 Check NVIDIA Driver and GPU Status
```bash
nvidia-smi
```

### 1.2 Monitor GPU Usage in Real Time (Optional)
```bash
watch -n 1 nvidia-smi
```

### 1.3 Check CUDA Version (Optional)
```bash
nvcc --version
```

### 1.4 Verify TensorFlow GPU Visibility (Quick Check)
```bash
python - << 'EOF'
import tensorflow as tf
print(tf.config.list_physical_devices('GPU'))
EOF
```

> Note: Terminal commands cannot configure TensorFlow GPU memory behavior. They are used only for verification and debugging.

---

## 2. Python Configuration (TensorFlow GPU Memory Growth)

TensorFlow may allocate all available GPU memory by default. To prevent this behavior and allow shared GPU usage, enable memory growth as shown below.

### 2.1 Standard Configuration

```python
import tensorflow as tf

# List available GPUs
gpus = tf.config.list_physical_devices('GPU')

if gpus:
    try:
        # Enable memory growth for each GPU
        for gpu in gpus:
            tf.config.experimental.set_memory_growth(gpu, True)
        print(f"Memory growth enabled for {len(gpus)} GPU(s).")
    except RuntimeError as e:
        print(e)
else:
    print("No GPU detected.")
```

---

### 2.2 Recommended Stable Configuration (With Logging)

```python
import tensorflow as tf

gpus = tf.config.list_physical_devices("GPU")

if not gpus:
    raise RuntimeError("No GPU detected by TensorFlow.")

for gpu in gpus:
    tf.config.experimental.set_memory_growth(gpu, True)

logical_gpus = tf.config.list_logical_devices("GPU")
print(f"Physical GPUs: {len(gpus)}")
print(f"Logical GPUs: {len(logical_gpus)}")
```

---

## 3. Important Notes

- GPU memory growth **must be configured before** any TensorFlow model, tensor, or session is created.
- If configured too late, TensorFlow will raise:
  ```
  RuntimeError: Physical devices cannot be modified after being initialized
  ```
- This configuration is strongly recommended for:
  - Multi-user GPU servers
  - Shared research environments
  - Preventing unnecessary Out-Of-Memory (OOM) errors

---

## 4. When to Use Each Method

| Scenario | Terminal | Python |
|--------|----------|--------|
| Driver / GPU troubleshooting | ✔ | |
| CUDA environment verification | ✔ | |
| TensorFlow GPU memory control | | ✔ |
| Multi-user GPU environment | | ✔ |

---

End of document.
