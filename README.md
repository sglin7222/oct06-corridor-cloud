# Oct06_r001 走廊點雲 / Corridor point cloud

ZED 2i 推車走廊錄影（去程 16.7 m、倒車回程 17.0 m，未迴轉）以離線攝影測量管線重建的彩色點雲：
關鍵影格 162 張 → COLMAP rig bundle adjustment（左右基線固定）→ NEURAL_PLUS 深度重貼，5 mm 體素、取 10 m 內。
網頁版抽稀為 2 cm（122 萬點）與 1 cm（560 萬點）兩檔，three.js 顯示。

資料格式：`<key>_cloud.json` 描述外框與分塊，每塊 `.txt` 為 base64 的「uint16 xyz ×n ＋ uint8 rgb ×n」。

Colour point cloud of a corridor recorded with a ZED 2i on a cart (forward 16.7 m, reversed 17.0 m without turning), reconstructed offline:
162 keyframes → COLMAP rig bundle adjustment → NEURAL_PLUS depth re-projection, 5 mm voxels within 10 m. Web copy downsampled to 2 cm / 1 cm, rendered with three.js.

## 2026-10-08 新增：正規貼圖網格（texrecon）

同一組 COLMAP 位姿：162 張深度圖 TSDF 融合（1 cm 體素）→ Marching Cubes → Taubin 平滑 → 簡化到 150 萬面 → mvs-texturing（texrecon）逐面選圖＋接縫勻色。
檔案 `tex_mesh.json` / `tex_mesh_NN.txt` / `tex_tex.jpg`（84 張貼圖集縮排成一張 4096²）。
取代先前自寫的逐照片硬切版（接縫條紋多），舊版已移除。

Textured mesh via TSDF fusion (Open3D VoxelBlockGrid, 1 cm) + mvs-texturing (Waechter et al. 2014), same COLMAP poses; 84 atlases repacked into one 4096² JPEG for the web.
