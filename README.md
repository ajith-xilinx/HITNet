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

##### For Input Shape 540x960
```
cd /workspace/Vitis-AI/HITNet
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu
```
##### For Input Shape 992x1420

```
cd /workspace/Vitis-AI/HITNet
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu
```

### Install Vitis-AI 2.5 

