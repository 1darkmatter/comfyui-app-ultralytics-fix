# 🧠 ComfyUI + Ultralytics Fix  
**Patching YOLOv8/v10 Compatibility in the Impact Subpack**

> PyTorch 2.6+ broke `torch.load()` for YOLO checkpoints. This repo patches it like a pro.

---

## 🔥 What’s the Problem?

If you’re using **Ultralytics YOLO models** (v8 or v10) in ComfyUI — especially with the [Impact Pack](https://github.com/ltdrdata/ComfyUI-Impact-Pack) — you’ve likely hit one of these:

