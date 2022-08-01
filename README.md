# HITNet

## Run HITNet Inference on VCK5000 with Vitis-AI 2.5 

### HITNet: Hierarchical Iterative Tile Refinement Network for Real-time Stereo Matching


### Step 1 : ( On Host System ) 

```
sudo service docker restart 
sudo docker start vitis_ai_2.5 
sudo docker exec -it vitisai_2.5 start
```

### Step 2 : ( Inside Docker )

##### Set VCK5000 Environment 

```
source /workspace/setup/vck5000/setup.sh DPUCVDX8H_8pe_normal
```
##### Activate Conda Environment 
```
conda activate 
```
##### To Run Calibration & Generate INT8 Model for model of Input shape 992 x 1420
##### This Step will take approx 12 + 3 Minutes, for calib & model generation respectively 
```
cd /workspace/Vitis-AI/HITNet
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu # 
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu # Takes 
```

##### To Run Calibration & Generate INT8 Model for model of Input shape 540 x 960  
```
cd /workspace/Vitis-AI/HITNet
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu # 
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu # Takes 
