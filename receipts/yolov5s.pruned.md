<!--
Copyright (c) 2021 - present / Neuralmagic, Inc. All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

---
# General Hyperparams
num_epochs: &num_epochs 240

# Pruning Hyperparams
pruning_start_epoch: &pruning_start_epoch 4
pruning_end_epoch: &pruning_end_epoch 100

pruning_modifiers:
  - !GMPruningModifier
    params: __ALL_PRUNABLE__
    init_sparsity: 0.05
    final_sparsity: 0.75
    start_epoch: *pruning_start_epoch
    end_epoch: *pruning_end_epoch
    update_frequency: 1.0
---

# YOLOv5s Pruned

This recipe creates a sparse, [YOLOv5s](https://github.com/ultralytics/yolov5) model that achieves 96% recovery of its baseline accuracy on the COCO dataset (0.556 mAP@0.5 baseline vs 0.534 mAP@0.5 for this recipe).
Training was done using 4 GPUs at half precision with a total batch size of 256 using the [SparseML integration with ultralytics/yolov5](https://github.com/neuralmagic/sparseml/tree/main/integrations/ultralytics-yolov5).

When running, adjust hyperparameters based on training environment and dataset.

## Training

To set up the training environment, follow the instructions on the [integration README](https://github.com/neuralmagic/sparseml/blob/main/integrations/ultralytics-yolov5/README.md).
Using the given training script from the `yolov5` directory the following command can be used to launch this recipe. 
Adjust the script command for your GPU device setup. 
Ultralytics supports both DataParallel and DDP.

*script command:*

```
python train.py \
    --cfg ../models/yolov5s.yaml \
    --weights PRETRAINED_WEIGHTS \
    --data coco.yaml \
    --hyp data/hyp.lp.yaml \
    --recipe ../recipes/yolov5s.pruned.md \
```
