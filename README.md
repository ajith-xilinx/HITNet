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
#### To run Synthetic Quantization : 

##### To run Qunatization for the model with Input shape 540x960 : 
##### Go to the HitNet directory
```
cd /workspace/Vitis-AI/HITNet
```

##### Activate Conda Environment 
```
conda activate 
```
##### Run Calibration. This Step will take approx 12 Minutes 
```
pyton synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu # 
```
##### Generate the Quantized Model. This Step will take approx 3 Minutes 

```
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu # Takes 
```

##### To run Qunatization for the model with Input shape 540x960, repeat Step 2 with " --h 540 --w 960 "  

