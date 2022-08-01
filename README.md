# HITNet

## Run HITNet Inference on VCK5000 with Vitis-AI 2.5 

### HITNet: Hierarchical Iterative Tile Refinement Network for Real-time Stereo Matching


### Step 1 : On Host System : 

```
sudo service docker restart 
sudo docker start vitis_ai_2.5 
sudo docker exec -it vitisai_2.5 bash
```

### Step 2 : Inside Docker : 

##### Set VCK5000 Environment 

```
source /workspace/setup/vck5000/setup.sh DPUCVDX8H_8pe_normal
```
##### Activate VITIS-AI Pytorch Conda Environment 
```
conda activate 
```
##### To Run Synthetic Calibration & Generate INT8 Model for model of Input shape 992 x 1420 ( This Step will take approx 12 + 3 Minutes ) 
```
cd /workspace/Vitis-AI/HITNet
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu  
```

##### To Run Synthetic Calibration & Generate INT8 Model for model of Input shape 540 x 960 ( This Step will take approx 5 + 3 Minutes ) 
```
cd /workspace/Vitis-AI/HITNet
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu 
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu 
```

##### Activate VITIS-AI WeGo Conda Environment to Run Inference 
```
conda activate 
```
##### To Run Inference of HITNet model on Synthetic Input of shape 992 x 1420
```
python synthetic_inference.py 
```
##### To Run Inference of HITNet model on Synthetic Input of shape 540 x 960
```
python synthetic_inference.py 
```
