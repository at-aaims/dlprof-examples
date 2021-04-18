#!/bin/bash
#BSUB -P stf011
#BSUB -J imagenet
#BSUB -o imagenet.optimized.dlprof.o%J
#BSUB -W 0:20
#BSUB -nnodes 1
#BSUB -alloc_flags "nvme smt4"
##BSUB -q debug
# End LSF directives and begin shell commands

nnodes=$(cat ${LSB_DJOB_HOSTFILE} | sort | uniq | grep -v login | grep -v batch | wc -l)

REPO_ROOT=$(pwd)/../DeepLearningExamples/PyTorch/Classification/ConvNets/
DATA_DIR=$(pwd)

# Load required modules
module use /sw/aaims/summit/modulefiles
module load dlprof

# Step into DeepLearningExamples directory and run
cd ${REPO_ROOT}

jsrun --smpiargs="-disable_gpu_hooks" -n${nnodes} -a1 -c42 -g1 -r1 --bind=proportional-packed:7 --launch_distribution=packed \
    bash -c "\
    source ${DATA_DIR}/export_DDP_envvars.sh && \
    dlprof --mode="pytorch" python -u main.py \
    --arch resnet50 \
    -j 28 \
    -p 10 \
    -b 128\
    --training-only \
    --raport-file ${DATA_DIR}/benchmark.optimized.json \
    --epochs 1 \
    --prof 100 \
    --no-checkpoints \
    --data-backend syntetic \
    --amp \
    --memory-format nhwc \
    ${DATA_DIR}
    "

mv nsys_profile.* event_files ${DATA_DIR} 
