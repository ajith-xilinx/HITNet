# HITNet

## Run HITNet Inference on VCK5000 with Vitis-AI 2.5 

### HITNet: Hierarchical Iterative Tile Refinement Network for Real-time Stereo Matching

```
sudo service docker restart 
sudo docker start vitis_ai_2.5 
sudo docker exec -it vitisai_2.5 start
```

Inside Docker : 

#### To run Synthetic Quantization : 

##### To run Qunatization for the model with Input shape 540x960 : 
##### Go to the HitNet directory
```
cd /workspace/Vitis-AI/HITNet
```
##### Run Calibration. This Step will take approx 12 Minutes 
```
pyton synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu # 
```
##### Generate the Quantized Model. This Step will take approx 3 Minutes 

```
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu # Takes 
```


##### To Quantize the model with Input shape 992x1420 : 
##### Go to the HitNet directory
```
cd /workspace/Vitis-AI/HITNet
```
##### Run Calibration. This Step will take approx 12 Minutes 
```
pyton synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu # 
```
##### Generate the Quantized Model. This Step will take approx 3 Minutes 

```
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu # Takes 
```

