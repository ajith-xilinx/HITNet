# HITNet

## Run HITNet Inference on VCK5000 with Vitis-AI 2.5 

### HITNet: Hierarchical Iterative Tile Refinement Network for Real-time Stereo Matching

```
sudo service docker restart <br> 
sudo docker start vitisai_2.5 
sudo docker exec -it vitisai_2.5 start
```
Inside Docker : 

```
cd /workspace/Vitis-AI/HITNet
python new_test_script.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu
python new_test_script.py --nndct_leaky_relu_approximate False --quant_mode calib --use_cpu
```

### Install Vitis-AI 2.5 

