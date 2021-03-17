Isolated Sign Language Recognition with Multi-modal Models
=========
By [Songyao Jiang](https://www.songyaojiang.com/), Bin Sun, [Lichen Wang](https://sites.google.com/site/lichenwang123/), [Yue Bai](https://yueb17.github.io/), [Kunpeng Li](https://kunpengli1994.github.io/) and [Yun Fu](http://www1.ece.neu.edu/~yunfu/).

[Smile Lab @ Northeastern University](https://web.northeastern.edu/smilelab/)

[![PWC](https://img.shields.io/endpoint.svg?url=https://paperswithcode.com/badge/skeleton-based-sign-language-recognition/sign-language-recognition-on-autsl)](https://paperswithcode.com/sota/sign-language-recognition-on-autsl?p=skeleton-based-sign-language-recognition)
----
This repo contains the code to reproduce our results on [2021 Looking at People Large Scale Signer Independent Isolated SLR CVPR Challenge](http://chalearnlap.cvc.uab.es/challenge/43/description/).

In this repo, we provide trained models and a step-by-step instruction to train, test our models and reproduce our final results. We also provide a docker file for quick deployment of our environment setup. 
We are going to submit a paper to the workshop associated with this challenge to provide more details about our work. Please consider cite that paper if you find this repo useful in your research.

## News

[2021/03/15] The code is released to public on [GitHub](https://github.com/jackyjsy/CVPR21Chal-SLR).

[2021/03/11] Our team (smilelab2021) ranked 1st in both tracks and here's the links to the leaderboards: 

* [RGB](https://competitions.codalab.org/competitions/27901#results)

* [RGBD](https://competitions.codalab.org/competitions/27902#results)



## Data Preparation and Processing

There are six modalities in total: skeleton, skeleton features, rgb frames, flow color, hha and flow depth. 

We provide preprocessed test data of all modalities to reproduce our submitted results.

[Download via Google Drive](https://drive.google.com/drive/folders/1Skz9sbQUhciPUS4Ta76w5ZraCN5cYeuc?usp=sharing)

1. Please put original train, val, test videos in data folder as

    --data/

    ----train/

    ----val/

    ----test/

2. Follow the [data_processs/readme.md](data_processs/readme.md) to process the data.

3. Use TPose/data_process to extract wholebody pose features.



## Requirements and Docker Images

The code is written using Anaconda Python >= 3.6 and Pytorch 1.7 with OpenCV.

Detailed enviroment requirment can be found in requirement.txt in each code folder.

For convenience, we provide a Nvidia docker image to run our code. 

[Download Docker Image](https://drive.google.com/file/d/1xDRhWW8mnZFXZw0c7pQZkDktHCmM9vL2/view?usp=sharing)

## Pretrained models
We provide pretrained models for all modalities to reproduce our submitted results. Please download them at and put them into corresponding folders.

[Download Pretrained Models](https://drive.google.com/drive/folders/1VcbTfnRa95XYxRB6JPdTiHvIzipqQeW3?usp=sharing)



## Reproducing the results submitted to CVPR21 Challenge
To test our pretrained model, please put them under each code folders and run the test code as instructed below. To ensemble the tested results and reproduce our final submission. Please copy all the results .pkl files to ensemble/ and follow the instruction to ensemble our final outputs.

For a step-by-step instruction, please see [reproduce.md](reproduce.md).

## Skeleton Keypoints
Skeleton modality can be trained, finetuned and tested using the code in GCN/ folder. Please follow the [GCN/readme.md](GCN/readme.md) instruction to prepare skeleton data into four streams (joint, bone, joint_motion, bone motion).

Basic usage:

    python main.py --config /path/to/config/file

To train, finetune and test our models, please change the config path to corresponding config files. Detailed instruction can be found in [GCN/readme.md](GCN/readme.md)

## Skeleton Feature (TPose)
Follow the instruction in TPose/ to prepare the data, train and test the model.

## RGB Frames 
The RGB frames modality can be trained, finetuned and tested using the following commands in Conv3D/ folder.  

    python Sign_Isolated_Conv3D_clip.py

    python Sign_Isolated_Conv3D_clip_finetune.py

    python Sign_Isolated_Conv3D_clip_test.py

Detailed instruction can be found in Conv3D/readme.md

## Flow Color 
The Flow Color modality can be trained, finetuned and tested using the following commands in Conv3D/ folder. 

    python Sign_Isolated_Conv3D_flow_clip.py

    python Sign_Isolated_Conv3D_flow_clip_funtine.py

    python Sign_Isolated_Conv3D_flow_clip_test.py

Detailed instruction can be found in [Conv3D/readme.md](Conv3D/readme.md)

## HHA
The HHA modality can be trained, finetuned and tested using the following commands in Conv3D/ folder. 

    python Sign_Isolated_Conv3D_hha_clip_mask.py

    python Sign_Isolated_Conv3D_hha_clip_mask_finetune.py

    python Sign_Isolated_Conv3D_hha_clip_mask_test.py

Detailed instruction can be found in [Conv3D/readme.md](Conv3D/readme.md)

## Flow Depth 
The Flow Depth modality can be trained, finetuned and tested using the following commands in Conv3D/ folder. 

    python Sign_Isolated_Conv3D_depth_flow_clip.py

    python Sign_Isolated_Conv3D_depth_flow_clip_finetune.py

    python Sign_Isolated_Conv3D_depth_flow_clip_test.py

Detailed instruction can be found in [Conv3D/readme.md](Conv3D/readme.md)

## Model Ensemble
For both RGB and RGBD track, the tested results of all modalities need to be ensemble together to generate the final results. 

1. For RGB track, we use the results from skeleton, skeleton feature, rgb, and flow color modalities to ensemble the final results. 

   a. Test the model using newly trained weights or provided pretrained weights.

   b. Copy all the test results to ensemble folder and rename them as their modality names.

   c. Ensemble GCN results from joint, bone, joint motion, bone motion streams in gcn/ .

        python ensemble_wo_val.py; python ensemble_finetune.py

   c. Copy test_gcn_w_val_finetune.pkl to ensemble/. Copy RGB, TPose and optical flow results to ensemble/. Ensemble final prediction.
        
        python ensemble_multimodal_rgb.py
        
    Final predictions are saved in predictions.csv

2. For RGBD track, we use the results from skeleton, skeleton feature, rgb, flow color, hha and flow depth modalities to ensemble the final results. 
    a. copy hha and flow depth modalities to ensemble/ folder, then
    
        python ensemble_multimodal_rgb.py
    

To reproduce our results in CVPR21Challenge, we provide .pkl files to ensemble and obtain our final submitted predictions. Detailed instruction can be find in [ensemble/readme.md](ensemble/readme.md)

## Reference

https://github.com/Sun1992/T-Pose-model-for-SLR

https://github.com/jin-s13/COCO-WholeBody

https://github.com/open-mmlab/mmpose

https://github.com/0aqz0/SLR

https://github.com/kchengiva/DecoupleGCN-DropGraph

## License
This project is released under the Apache 2.0 license with exceptions:

Commercial usage is prohibited.

Published versions (changed or unchanged) must include a reference to the origin of the code (my extension source code).

## Citation
If you find this project useful in your research, please cite our preprint fact sheet version:

```
@article{jiang2021skeleton,
  title={Skeleton Based Sign Language Recognition Using Whole-body Keypoints},
  author={Jiang, Songyao and Sun, Bin and Wang, Lichen and Bai, Yue and Li, Kunpeng and Fu, Yun},
  journal={arXiv preprint arXiv:2103.08833},
  year={2021}
}
```

The workshop version of our paper will be updated here soon.