# PyTorch GPU Usage Operational Guidelines

This document defines the standard operating procedure (SOP) for verifying GPU availability and managing GPU memory behavior when using PyTorch on Linux systems with NVIDIA GPUs.

> Note: Unlike TensorFlow, PyTorch uses **dynamic GPU memory allocation by default** and typically does **not** require explicit memory growth configuration.

---

## 1. Terminal Checks (System-Level Verification)

Use the following commands to ensure that the NVIDIA driver and GPU hardware are functioning correctly.

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

---

## 2. Python Checks (PyTorch GPU Availability)

### 2.1 Basic GPU Availability Check

```python
import torch

print("CUDA available:", torch.cuda.is_available())
print("Number of GPUs:", torch.cuda.device_count())

if torch.cuda.is_available():
    print("Current GPU:", torch.cuda.get_device_name(0))
```

---

### 2.2 Explicit Device Selection (Recommended Practice)

```python
import torch

device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print("Using device:", device)
```

Usage example:
```python
model = model.to(device)
inputs = inputs.to(device)
```

---

## 3. GPU Memory Behavior in PyTorch

### 3.1 Default Behavior (Important Concept)

- PyTorch **allocates GPU memory on demand**
- Memory is cached by the PyTorch allocator for performance
- Released tensors do **not** immediately return memory to the OS
- This behavior is **normal** and expected

---

### 3.2 Clearing Cached GPU Memory (Optional)

```python
import torch
torch.cuda.empty_cache()
```

> This does **not** free memory used by active tensors  
> It only releases unused cached memory back to the CUDA allocator

---

### 3.3 Monitoring PyTorch GPU Memory Usage

```python
import torch

print("Allocated:", torch.cuda.memory_allocated() / 1024**2, "MB")
print("Reserved :", torch.cuda.memory_reserved() / 1024**2, "MB")
```

---

## 4. Advanced (Optional): Limiting GPU Memory Usage

PyTorch does not enforce hard memory limits by default, but you may apply soft limits if necessary.

### 4.1 Limit Per-Process GPU Memory Fraction

```python
import torch

torch.cuda.set_per_process_memory_fraction(0.5, device=0)
```

> Limits the process to approximately 50% of the selected GPU memory  
> Use with caution in multi-user environments

---

## 5. Environment Variable Control (Recommended for Servers)

### 5.1 Restrict Visible GPUs

```bash
export CUDA_VISIBLE_DEVICES=0
```

```bash
# Multiple GPUs
export CUDA_VISIBLE_DEVICES=0,1
```

---

## 6. When to Use Each Control Method

| Scenario | Method |
|--------|--------|
| Check driver / GPU health | Terminal (`nvidia-smi`) |
| Verify PyTorch GPU access | `torch.cuda.is_available()` |
| Avoid OOM in large models | Reduce batch size |
| Multi-user GPU servers | `CUDA_VISIBLE_DEVICES` |
| Debug memory behavior | `memory_allocated / reserved` |

---

## 7. Key Takeaways

- PyTorch does **not** require memory growth configuration
- GPU memory behavior is dynamic and allocator-managed
- Apparent "memory not released" issues are usually allocator caching
- Explicit device management is best practice

---

End of document.
