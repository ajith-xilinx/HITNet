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

```
cd /workspace/Vitis-AI/HITNet
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu
python synthetic_quantize.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu
```

##### To run Qunatization for shape 992x1420, run the above commands with arguments as "--h 992 --w 1420"

### Install Vitis-AI 2.5 

