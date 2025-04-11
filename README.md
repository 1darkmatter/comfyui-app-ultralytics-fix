# ComfyUI APP – Ultralytics Model Fix (Impact Pack Compatible)

**Patch for enabling YOLOv8/v10 model support in ComfyUI APP + Impact Pack under PyTorch 2.6+**

---

## 📌 Context

If you're using the **ComfyUI APP** (downloadable installer version, not the GitHub clone) along with the [Impact Pack](https://github.com/ltdrdata/ComfyUI-Impact-Pack), you may encounter model loading issues when trying to use **Ultralytics YOLOv8 or YOLOv10** checkpoints (e.g., for detection or segmentation tasks).

This is due to **PyTorch’s enhanced security features** in version 2.6+, which block necessary global functions like `getattr` by default during model loading.

---

## ❌ The Problem

When attempting to use a YOLO model inside the APP (especially via Ultralytics detailers), you may see:

```python
_pickle.UnpicklingError: Weights only load failed. GLOBAL 'getattr' was not an allowed global by default.
This is because torch.load() now enforces weights_only=True by default — and YOLO checkpoints require getattr() during deserialization, which isn’t safe-listed.

✅ The Fix
This patch safely re-enables required globals for YOLO model compatibility inside the ComfyUI APP.

---

##This is because torch.load() now enforces weights_only=True by default — and YOLO checkpoints require getattr() during deserialization, which isn’t safe-listed.

✅ The Fix
This patch safely re-enables required globals for YOLO model compatibility inside the ComfyUI APP, without compromising the integrity of PyTorch’s security model.

You simply need to add a patch to the file that wraps torch.load() — typically found in the comfyui-impact-subpack/modules/ directory.

You can use the minimal fix snippet here:
🔗 comfyui-impact-ultralytics-patch

Or refer to a full example of a patched module:
📄 full_subcore.py

🔐 About This Patch
Fully supports Ultralytics YOLOv8 and YOLOv10

Maintains compatibility with ComfyUI App + Impact Pack

Avoids unsafe global exposure — only getattr is explicitly permitted

Designed specifically for PyTorch 2.6+ changes

🔧 Compatibility
ComfyUI APP: Official installed version

Impact Pack: Latest build

PyTorch: 2.6+

YOLO Models: v8, v10 (Ultralytics format)
