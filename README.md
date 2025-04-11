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

🔧 Installation (for ComfyUI APP users)
Locate your installed ComfyUI APP directory.

Example paths:

Windows:

makefile
Copy
Edit
C:\Users\YourName\ComfyUI_windows_portable\ComfyUI\custom_nodes\
macOS:

swift
Copy
Edit
~/Applications/ComfyUI.app/Contents/Resources/ComfyUI/custom_nodes/
Inside the custom_nodes/comfyui-impact-subpack/modules/ folder, do one of the following:

Replace the existing subcore.py file with full_subcore.py
(Includes safe loading logic + model handling code)

Or, if you only want the load patch, drop in
comfyui-impact-ultralytics-patch
(Just the patch logic to override torch.load())

Restart the ComfyUI APP.

🧠 Why This Happens
PyTorch ≥ 2.6 restricts globals during torch.load() for security

Ultralytics YOLO relies on getattr() inside checkpoint metadata

getattr must be explicitly allowed, or the model load will fail

ComfyUI APP does not currently patch this internally

📋 Tested On
ComfyUI APP 2024.04+

PyTorch 2.6–2.8 (tested on nightly + stable)

YOLOv8 / YOLOv10 checkpoints (.pt)

Impact Pack + Subpack

macOS MPS / Windows CUDA

🛡️ Security Notes
This patch respects PyTorch’s weights_only policy and only re-adds minimal, necessary global functions (getattr) required by YOLO models. It does not open arbitrary code execution surfaces.

License
This patch is distributed under the MIT License. See LICENSE for more info.

yaml
Copy
Edit

---

Want a version of this formatted as a PNG graphic too? Or a one-liner installation guide for pinning it in release notes or Civitai post banners?
