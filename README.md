# HITNet 

## HITNet Inference on VCK5000 with Vitis-AI 2.5

#### HITNet: [Hierarchical Iterative Tile Refinement Network for Real-time Stereo Matching](https://arxiv.org/pdf/2007.12140.pdf)


### Step 1 : On Host System : 
------------------------------------------------------------

```
sudo service docker restart 
sudo docker start vitis_ai_2.5 
sudo docker exec -it vitisai_2.5 bash
```

### Step 2 : Inside Docker : 
------------------------------------------------------------

##### Set VCK5000 Environment 

```
source /workspace/setup/vck5000/setup.sh DPUCVDX8H_8pe_normal
```

#### 2.1 Qunatize - Synthetic Data :
------------------------------------------------------------
##### Activate VITIS-AI Pytorch Conda Environment 
```
conda activate vitis-ai-pytorch 
```
##### To Run Synthetic Calibration & Generate INT8 Model for Input shape 992 x 1420 : ( This Step will take approx 12 + 3 Minutes ) 
```
cd /workspace/Vitis-AI/HITNet
```
```
python synthetic_quantize.py --nndct_leaky_relu_approximate False --use_cpu --quant_mode calib 
```
```
python synthetic_quantize.py --nndct_leaky_relu_approximate False --use_cpu --quant_mode test 
```

##### To Run Synthetic Calibration & Generate INT8 Model for Input shape 540 x 960 : ( This Step will take approx 5 + 2 Minutes ) 
```
cd /workspace/Vitis-AI/HITNet
```
```
python synthetic_quantize.py --nndct_leaky_relu_approximate False --use_cpu --quant_mode calib 
```
```
python synthetic_quantize.py --nndct_leaky_relu_approximate False --use_cpu --quant_mode test 
```
#### 2.2 Compile & Run Inference  - Synthetic Data :
------------------------------------------------------------
##### Activate VITIS-AI WeGo Conda Environment to Run Inference 
```
conda activate vitis-ai-wego-torch 
```
##### To Run Inference of HITNet model on Synthetic Input of shape 992 x 1420 : ( This Step will take approx 25 Minutes )
```
python synthetic_inference.py --model_path quant_small/PredictModel_int.pt --shape=1,3,992,1420 \
                              --wego_subgraph_min_ops_number=1 --device=wego
```
##### To Run Inference of HITNet model on Synthetic Input of shape 540 x 960 ( This Step will take approx 5 Minutes )
```
python synthetic_inference.py --model_path quant_small/PredictModel_int.pt --shape=1,3,540,960 \
                              --wego_subgraph_min_ops_number=1 --device=wego
```

###### To Run Inference of HITNet model, run the above commands with "--device=cpu"

<br>

#### 3.1 Qunatize - Real Data :
------------------------------------------------------------
##### Activate VITIS-AI Pytorch Conda Environment 
```
conda activate vitis-ai-pytorch 
```
##### To Run Synthetic Calibration & Generate INT8 Model for Input shape 540 x 960 ( This Step will take approx 12 + 3 Minutes ) 
```
cd /workspace/Vitis-AI/HITNet
```
```
python quantize.py --model HITNetXL_SF --ckpt ckpt/hitnet_xl_sf_finalpass_from_tf.ckpt --data_type SceneFlow \
                   --data_root_val data/subset_of_sf --data_list_val data/subset_of_sf/sceneflow_test_sub.list \
                   --nndct_leaky_relu_approximate False --use_cpu --quant_mode calib
```
```
python quantize.py --model HITNetXL_SF --ckpt ckpt/hitnet_xl_sf_finalpass_from_tf.ckpt --data_type SceneFlow \
                   --data_root_val data/subset_of_sf --data_list_val data/subset_of_sf/sceneflow_test_sub.list \
                   --nndct_leaky_relu_approximate False --use_cpu --quant_mode test
```
#### 3.2 Compile & Run Inference  - Real Data :
------------------------------------------------------------
##### Activate VITIS-AI WeGo Conda Environment to Run Inference 
```
conda activate vitis-ai-wego-torch
```
##### To Run Inference of HITNet model on Synthetic Input of shape 992 x 1420
```
python inference.py 
```
