---
title: "安装PointNeRF相关环境"
date: 2023-01-03
header: 
    teaser: "dd.[ng"
excerpt: <p> 以前安装过一次PointNeRF的环境，但是这次在自己的服务器上又要安装一遍，记忆有点模糊了，所以决定以后的实验一定都要做一个记录。感觉这种重复做轮子的事情很麻烦，所以记录下来，第三次安装环境的时候可以查看 </p>
---

### 论文的地址 
论文的地址在 <a href="https://github.com/Xharlie/pointnerf">PointNeRF</a>。

## 环境安装

### requirement.txt
``` bash
pip install -r requirement.txt
```

### 修改pip的源为清华源
```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

### 安装相应的环境库
```bash
pip install opencv-python
pip install imageio
pip install tqdm scipy matplotlib
pip install six kornia warmup_scheduler
pip install scikit-image tensorboardX lpips open3d
pip install plyfile h5py
```

### 安装pycuda
参考的教程是pycuda的<a href="https://wiki.tiker.net/PyCuda/Installation/Linux/#step-1-download-and-unpack-pycuda">官方文档</a>

```bash
python configure.py --cuda-root=/usr/local/cuda
sudo make install

pip install pytest
cd test 
python test_driver.py
python test_cumath.py
python test_gpuarray.py

# https://github.com/mapillary/inplace_abn
pip install torch==1.12.1+cu116 torchvision==0.13.1+cu116 torchaudio==0.12.1 --extra-index-url https://download.pytorch.org/whl/cu116
pip install inplace-abn
pip install torch-scatter -f https://data.pyg.org/whl/torch-1.12.1+cu116.html
# 非常重要
pip install numpy==1.23.0
```

### 运行程序
```python
python train_ft.py --experiment my_materials --scan materials --data_root ../data_src/nerf_synthetic/ --dataset_name nerf_synth360_ft --model mvs_points_volumetric --which_render_func radiance --which_blend_func alpha --out_channels 4 --num_pos_freqs 10 --num_viewdir_freqs 4 --random_sample random --random_sample_size 60 --batch_size 1 --plr 0.002 --lr 0.0005 --lr_policy iter_exponential_decay --lr_decay_iters 1000000 --lr_decay_exp 0.1 --gpu_ids 0 --checkpoints_dir ../checkpoints/nerfsynth/ --save_iter_freq 10000 --niter 10000 --niter_decay 10000 --n_threads 1 --pin_data_in_memory 1 --train_and_test 0 --test_num 10 --test_freq 10000 --test_num_step 10 --test_color_loss_items coarse_raycolor ray_miss_coarse_raycolor ray_masked_coarse_raycolor --print_freq 40 --bg_color white --split train --which_ray_generation near_far_linear --near_plane 2.0 --far_plane 6.0 --dir_norm 0 --which_tonemap_func off --load_points 0 --resume_dir ../checkpoints/init/dtu_dgt_d012_img0123_conf_agg2_32_dirclr20 --resume_iter best --feature_init_method rand --agg_axis_weight 1. 1. 1. --agg_distance_kernel linear --radius_limit_scale 4 --depth_limit_scale 0 --vscale 2 2 2 --kernel_size 3 3 3 --SR 80 --K 8 --P 9 --NN 2 --agg_feat_xyz_mode None --agg_alpha_xyz_mode None --agg_color_xyz_mode None --save_point_freq 10000 --raydist_mode_unit 1 --agg_dist_pers 20 --agg_intrp_order 2 --shading_feature_mlp_layer0 1 --shading_feature_mlp_layer1 2 --shading_feature_mlp_layer2 0 --shading_feature_mlp_layer3 2 --shading_feature_num 256 --dist_xyz_freq 5 --shpnt_jitter uniform --shading_alpha_mlp_layer 1 --shading_color_mlp_layer 4 --which_agg_model viewmlp --color_loss_weights 1.0 0.0 0.0 --num_feat_freqs 3 --dist_xyz_deno 0 --apply_pnt_mask 1 --point_features_dim 32 --color_loss_items ray_masked_coarse_raycolor ray_miss_coarse_raycolor coarse_raycolor --feedforward 0 --trgt_id 0 --depth_vid 0 --ref_vid 0 --manual_depth_view 1 --pre_d_est ../checkpoints/MVSNet/model_000014.ckpt --depth_occ 1 --manual_std_depth 0.0 --visual_items coarse_raycolor gt_image --appr_feature_str0 imgfeat_0_0123 dir_0 point_conf --init_view_num 3 --feat_grad 1 --conf_grad 1 --dir_grad 1 --color_grad 1 --sigma_grad 1 --lambda_grad 1 --normal_grad 1 --depth_conf_thresh 0.8 --bgmodel probe --vox_res 320 --act_type LeakyReLU --geo_cnsst_num 0 --point_conf_mode 1 --point_dir_mode 1 --point_color_mode 1 --point_sigma_mode 1 --point_lambda_mode 1 --point_normal_mode 1 --normview 0 --prune_thresh 0.1 --prune_iter -10001 --full_comb 1 --sparse_loss_weight 0 --default_conf 0.15 --prob_freq 10001 --prob_num_step 20 --prob_thresh 0.7 --prob_mul 0.4 --prob_kernel_size 3 3 3 --prob_tiers 30000 --alpha_range 0 --ranges -5.0 -5.0 -5.0 5.0 5.0 5.0 --vid 250000 --vsize 0.004 0.004 0.004 --wcoord_query 1 --max_o 930000 --zero_one_loss_items conf_coefficient --zero_one_loss_weights 0.0001 --prune_max_iter 130000 --far_thresh -1 --gen_embed_fn_type 2 --cloud_point_vis 0 --z_depth_dim 400 --build_normal_method open3d --maximum_step 0 --debug
```





