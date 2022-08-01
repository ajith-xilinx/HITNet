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
```
cd /workspace/Vitis-AI/HITNet
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu # Takes 12 Minutes Approx 
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu # Takes 
```
This will generate pt model at path  

##### To Quantize the model with Input shape 992x1420 : 
```
cd /workspace/Vitis-AI/HITNet
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu # Takes 12 Minutes Approx 
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu # Takes 12 Minutes Approx 
```
This will generate pt model at path

#### To run Inference with Syntehtic Data : 

