# HITNet 

## HITNet Inference on VCK5000 with Vitis-AI 2.5

#### HITNet : [Hierarchical Iterative Tile Refinement Network for Real-time Stereo Matching](https://arxiv.org/pdf/2007.12140.pdf)


### I. On Host System : 
------------------------------------------------------------

```
sudo service docker restart 
```
```
sudo docker start vitisai_2.5 
```
```
sudo docker exec -it vitisai_2.5 bash
```

### II. Inside Docker : 
------------------------------------------------------------

##### Set VCK5000 Environment :

```
source /workspace/setup/vck5000/setup.sh DPUCVDX8H_8pe_normal
```

#### 1.1 Qunatize - Synthetic Data :
------------------------------------------------------------
##### Activate VITIS-AI Pytorch Conda Environment :
```
conda activate vitis-ai-pytorch 
```

##### To Run Synthetic Calibration & Generate INT8 Model for Input shape 540 x 960 : ( This Step will take approx 5 + 2 Minutes ) 
```
cd /workspace/HITNET
```
```
python synthetic_quantize.py --ckpt ckpt/hitnet_xl_sf_finalpass_from_tf.ckpt --h 540 --w 960 --output_dir quant_model_540x960 \
                             --use_cpu --nndct_leaky_relu_approximate False --quant_mode calib 
```
```
python synthetic_quantize.py --ckpt ckpt/hitnet_xl_sf_finalpass_from_tf.ckpt --h 540 --w 960 --output_dir quant_model_540x960 \
                             --use_cpu --nndct_leaky_relu_approximate False --quant_mode test
```

##### To Run Synthetic Calibration & Generate INT8 Model for Input shape 992 x 1420 : ( This Step will take approx 12 + 3 Minutes ) 
```
cd /workspace/HITNET
```
```
python synthetic_quantize.py --ckpt ckpt/hitnet_xl_sf_finalpass_from_tf.ckpt --h 992 --w 1420 --output_dir quant_model_992x1420 \
                             --use_cpu --nndct_leaky_relu_approximate False --quant_mode calib
```
```
python synthetic_quantize.py --ckpt ckpt/hitnet_xl_sf_finalpass_from_tf.ckpt --h 992 --w 1420 --output_dir quant_model_992x1420 \
                             --use_cpu --nndct_leaky_relu_approximate False --quant_mode test
```

#### 1.2 Compile & Run Inference  - Synthetic Data :
------------------------------------------------------------
##### Activate VITIS-AI WeGo Conda Environment to Run Inference :
```
conda activate vitis-ai-wego-torch 
```

##### To Run Inference of Quantized HITNet model on Synthetic Input of shape 540 x 960 : ( This Step will take approx 5 Minutes )
```
python synthetic_inference.py --model quant_model_540x960/PredictModel_int.pt --shape=1,3,540,960 --device=wego
```

##### To Run Inference of Quantized HITNet model on Synthetic Input of shape 992 x 1420 : ( This Step will take approx 25 Minutes )
```
python synthetic_inference.py --model quant_model_992x1420/PredictModel_int.pt --shape=1,3,992,1420 --wego_subgraph_min_ops_number=1 --device=wego
```

###### To Run Inference of HITNet model, run the above commands with "--device=cpu"

<br>

#### 2.1 Qunatize - Real Data :
------------------------------------------------------------
##### Activate VITIS-AI Pytorch Conda Environment 
```
conda activate vitis-ai-pytorch 
```
##### To Run Synthetic Calibration & Generate INT8 Model for Input shape 540 x 960 : ( This Step will take approx 45 + 5 Minutes ) 
```
cd /workspace/HITNET
```
```
python quantize.py --ckpt ckpt/hitnet_xl_sf_finalpass_from_tf.ckpt --data_type SceneFlow --use_cpu \
                   --data_root_val data/subset_of_sf --data_list_val data/subset_of_sf/sceneflow_test_sub.list \
                   --nndct_leaky_relu_approximate False --output_dir real_quant_model_540x960 --quant_mode calib
```
```
python quantize.py --ckpt ckpt/hitnet_xl_sf_finalpass_from_tf.ckpt --data_type SceneFlow --use_cpu \
                   --data_root_val data/subset_of_sf --data_list_val data/subset_of_sf/sceneflow_test_sub.list \
                   --nndct_leaky_relu_approximate False --output_dir real_quant_model_540x960 --quant_mode test --deploy
```
#### 2.2 Compile & Run Inference  - Real Data :
------------------------------------------------------------
##### Activate VITIS-AI WeGo Conda Environment to Run Inference 
```
conda activate vitis-ai-wego-torch
```
##### To Run Inference of  HITNet model ( Quantized with Real Images ) with Synthetic Input shape 540 x 960 :
```
python synthetic_inference.py --model real_quant_model_540x960/PredictModel_int.pt --shape=1,3,540,960 --device=wego 
```

##### To Run Inference of  HITNet model ( Quantized with Real Images ) with Real Input shape 540 x 960 :
```
python inference.py --model real_quant_model_540x960/PredictModel_int.pt --shape=1,3,540,960 --device=wego \
                    --left=data/inputs/0006_left.png --right=data/inputs/0006_right.png --disp=data/inputs/0006.pfm

```

### Performance Issues and Planned Future Improvements :
There might be end2end performance issues when deploying the HitModel on the DPU using WeGO due to:

* Currently, DPU supports only a limit set of operators [Supported-Operators-and-DPU-Limitations](https://docs.xilinx.com/r/en-US/ug1414-vitis-ai/Supported-Operators-and-DPU-Limitations)
* For those operators that not supported by the DPU, WeGO will dispatch them to CPU side for execution to enable a flexiable deployment workflow. 
* Currently, HitNet model has many operators *not* supported by DPU: aten::clone, aten::sub, aten::constant_pad_nd, aten::lekay_relu(with factor 0.2), aten::slice etc. 
* For the model with input size 540x960, after partition, there will be 26 CPU subgrahs and 25 DPU subgraphs
* Due to this, the large amounts of data transfer overhead occurs between host and deivce
* For each DPU subgraph's execution, WeGO needs to perform transpose operations for both its input and output tensors
* This is consuming a lot of time as there are many DPU subgrahps and the size of input/output tensor size is large 
* This memory layout difference between DPU and PyTorch ( NHWC vs NCHW ) will degrade the performance further


### Future Improvments that are in Plan to boost the Performance of HITNet Model :

* Upgrade both DPU IP and xcompiler to cover more operator types
  * Eg. aten::leaky_relu ( with factor 0.2 ), aten::constant_pad_nd, etc.
  * This will drastically improve the performance as we reducing the Bandwidth load requirement between DPU & CPU
* Optimize Transpose operation by either supporting it in DPU directly or integrating WeGO with ZenDNN to leverage AMD-CPU highly-optimzed transpose kernels
