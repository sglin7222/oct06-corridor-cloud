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

`texfill_*`：補點版（每張深度圖的小破洞先用四周平面補、顏色取原影像像素，再融合貼圖）；`tex_*` 為 Raw。

2026-10-08 晚：GPU 融合網格（`gpu_*`）已移除。

## 2026-10-10 新增：3D 高斯潑濺（`splat.html`）

同一組 COLMAP 位姿與 162 張左影像（半解析度 640×360），gsplat 訓練 7000 次：75 萬個高斯、驗證 PSNR 31.85 dB、12 分鐘。
匯出成 `oct06.splat`（antimatter15 格式，去掉透明度 <0.02 後 66.6 萬個、21 MB，只存基礎色不含球諧），座標與點雲相同；
網頁用 [GaussianSplats3D](https://github.com/mkkellogg/GaussianSplats3D) 0.4.7 顯示。
沿錄影方向看幾乎等於照片；側轉 50°、低頭看反光地板會糊，那些角度錄影時沒拍到。

3D Gaussian Splatting of the same recording (gsplat, 7000 iters, 0.75 M Gaussians, test PSNR 31.85 dB), exported as `.splat` (SH0 only) in the same frame as the point cloud and rendered with GaussianSplats3D.
