<section>
  <h1>ComfyUI APP – Ultralytics Model Fix (Impact Pack Compatible)</h1>
  <p><strong>Patch for enabling YOLOv8/v10 model support in ComfyUI APP + Impact Pack under PyTorch 2.6+</strong></p>
  <hr/>

  <h2>📌 Context</h2>
  <p>
    If you're using the <strong>ComfyUI APP</strong> (downloadable installer version, not the GitHub clone)
    along with the
    <a href="https://github.com/ltdrdata/ComfyUI-Impact-Pack" target="_blank" rel="noopener">Impact Pack</a>,
    you may encounter model loading issues when trying to use
    <strong>Ultralytics YOLOv8 or YOLOv10</strong> checkpoints
    (e.g., for detection or segmentation tasks).
  </p>
  <p>
    This is due to <strong>PyTorch’s enhanced security features</strong> in version 2.6+,
    which block necessary global functions like <code>getattr</code> by default during model loading.
  </p>

  <h2>❌ The Problem</h2>
  <p>When attempting to use a YOLO model inside the APP (especially via Ultralytics detailers), you may see:</p>
  <pre><code class="language-python">
_pickle.UnpicklingError: Weights only load failed. GLOBAL 'getattr' was not an allowed global by default.
  </code></pre>
  <p>
    This happens because <code>torch.load()</code> now enforces <code>weights_only=True</code> by default —
    and YOLO checkpoints require <code>getattr()</code> during deserialization, which isn’t safe‑listed.
  </p>

  <h2>✅ The Fix</h2>
  <p>
    This patch safely re‑enables required globals for YOLO model compatibility inside the ComfyUI APP,
    without compromising the integrity of PyTorch’s security model.
  </p>
  <p>
    You simply need to add a patch to the file that wraps <code>torch.load()</code> —
    typically found in the <code>comfyui-impact-subpack/modules/</code> directory.
  </p>
  <ul>
    <li><a href="[🔗 comfyui-impact-ultralytics-patch](https://github.com/1darkmatter/comfyui-app-ultralytics-fix/blob/main/comfyui-impact-ultralytics-patch)">Minimal fix snippet</a></li>
    <li><a href="[📄 full_subcore.py](https://github.com/1darkmatter/comfyui-app-ultralytics-fix/blob/main/full_subcore.py)">Full example of a patched module</a></li>
  </ul>

  <h3>🔐 About This Patch</h3>
  <ul>
    <li>Fully supports Ultralytics YOLOv8 and YOLOv10</li>
    <li>Maintains compatibility with ComfyUI App + Impact Pack</li>
    <li>Avoids unsafe global exposure — only <code>getattr</code> is explicitly permitted</li>
    <li>Designed specifically for PyTorch 2.6+ changes</li>
  </ul>

  <h3>🔧 Compatibility</h3>
  <ul>
    <li><strong>ComfyUI APP:</strong> Official installed version</li>
    <li><strong>Impact Pack:</strong> Latest build</li>
    <li><strong>PyTorch:</strong> 2.6+</li>
    <li><strong>YOLO Models:</strong> v8, v10 (Ultralytics format)</li>
  </ul>
</section>
