# Dis_LSS
新论文构建与测试



## :hammer: Build environments


## :open_hands: Data preparation
下载nuscenes，然后给出一个详细的文件夹列表出来


对于Oerlock的backbone使用，参考对应的github链接


## :paperclip: Model Zoo
| Method | SID Depth | Loss Redesign | Backbone (Overlook) | val_iou | Δ IoU (%) | Config (YAML) | Checkpoint |  
|------|-----------|--------------|---------------------|--------|-----------|---------------|------------|  
| LSS (baseline) | ✗ | ✗ | ✗ | 0.3204 | +0.00% | `configs/lss.yaml` | [下载链接](#) |  
| + SID bins | ✓ | ✗ | ✗ | 0.3299 | +0.95% | `configs/lss_sid.yaml` | [Download](#) |  
| + Loss redesign | ✓ | ✓ | ✗ | **0.3879** | **+6.75%** | `configs/lss_sid_loss.yaml` | [Download (Best)](#) |  
| + Overlook (freeze) | ✓ | ✓ | ✓ (frozen) | ~0.30 | -2.04% | `configs/lss_sid_loss_overlook_freeze.yaml` | [Download](#) |  
| + Overlook (finetune) | ✓ | ✓ | ✓ (finetune) | 0.3824 | +6.20% | `configs/lss_sid_loss_overlook_finetune.yaml` | [Download](#) |  


## :rocket: Train from scratch



## :white_check_mark: Test



## :framed_picture: Visualize



## :pray: Acknowledeges
我参考了哪些库？ LSS，fiery以及Overlock


## :mortar_board: Citation引用我的文献





