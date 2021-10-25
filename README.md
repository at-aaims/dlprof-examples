# DLProf Examples on Summit 
This repo consists of usage examples of Nvidia DLProf on the Summit supercomputer.

[DLProf](https://docs.nvidia.com/deeplearning/frameworks/dlprof-user-guide/) is a tool for profiling deep learning models to help data scientists understand and improve performance of their models visually via Tensorboard or by analyzing text reports.

## Quickstart 

- use DLProf on Summit 
```bash
module use /sw/aaims/summit/modulefiles
module load dlprof
```
- refer this [blog](https://developer.nvidia.com/blog/profiling-and-optimizing-deep-neural-networks-with-dlprof-and-pyprof/) for more details.

### PyTorch Example

```bash
git clone --recursive https://github.com/at-aaims/dlprof-examples
cd dlprof-examples/DeepLearningExamples
git apply ../pytorch/ConvNets.patch
cd ../pytorch 
bsub prof.lsf
```

### Visualize Output

- install tensorboard plugin (for x86 only)
```bash
pip install nvidia-pyindex
pip install nvidia-tensorboard
pip install nvidia-tensorboard-plugin-dlprof
```
- use pre-installed env on Andes
```bash
module load python
source activate /gpfs/alpine/world-shared/stf011/junqi/dlprof-env
tensorboard --logdir /gpfs/alpine/world-shared/stf011/junqi/dlprof-env/event_files --host localhost
```
- port forward to local machine
```bash
http://localhost:6006
```
