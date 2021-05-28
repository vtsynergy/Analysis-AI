# Classification Model

The AI model in this folder is used to classify the 3D chest CT images to COVID-19 positive or negative case.

## Platform

We tested the framework on platform shown below:

Operating System: Linux

Architecture: x86_64

Distribution: Ubuntu

Version: 18.04

## Hardware requirements
One or more Nvidia GPUs. (Two or more GPUS are recommended for advanced analysis features.) This GPU version is only compatible with Nvidia GPUs having compute capability 6.0 or higher, i.e., any GPU from Pascal, Volta, Turing, Ampere, and beyond will work.

## Software requirements
Nvidia-docker and Nvidia Clara SDK are required to run the package. If you already have docker installed then user should be in the docker group. Otherwise, sudo access is needed to install the prerequisite. Nvidia Clara SDK requires Nvidia CUDA 11.0.194 and Nvidia Driver release 450 or later.

## Installation

**1. Install Nvidia-docker**

Install Nvidia-docker following the instruction [here](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker).

**2. Install Nvidia Clara SDK**

Install Nvidia Clara SDK following the instruction [here](https://docs.nvidia.com/clara/deploy/ClaraInstallation.html).

**3. Download Nvidia Clara-train-example and unzip**

3.1.Download Nvidia clara-train-example from [here](https://github.com/NVIDIA/clara-train-examples).

3.2. Unzip the downloaded file and there should be two folders:
![image](https://user-images.githubusercontent.com/31482058/114258892-ee2d7500-9997-11eb-831f-21d9c10e52a0.png)

**4. Model Download**

The pre-trained classification model could be download from NVIDIA NGC:

4.1. Go to the homepage of NVIDIA NGC [here](https://ngc.nvidia.com/signin) and Log in with an NVIDIA account. If you do not have an NVIDIA account, sign up first.

![image](https://user-images.githubusercontent.com/31482058/114258934-46fd0d80-9998-11eb-84ff-8dc16dca538c.png)

4.2. Choose “MODELS” at the top of the homepage, and click it.

![image](https://user-images.githubusercontent.com/31482058/114258951-5aa87400-9998-11eb-8b7a-82088720076c.png)

4.3. Type in “covid” and search.

![image](https://user-images.githubusercontent.com/31482058/114258957-6bf18080-9998-11eb-8828-e4a27c2ff34a.png)

4.4. Click “Download” to download the "clara_train_covid19_3d_ct_classification" packages.

![image](https://user-images.githubusercontent.com/31482058/114258966-7ca1f680-9998-11eb-8b5d-3e7dce27ac9e.png)

4.5. Extract the downloaded compressed file, and get several folders

![image](https://user-images.githubusercontent.com/31482058/114258972-87f52200-9998-11eb-90a0-cc597f7b033e.png)

4.6. Put the “enhancement_model” folder downloaded from DECT into this path: ~/clara-train-examples-master/enhancement_model

4.7. Create a folder under the root of Nvidia clara-train-example folder: “classify_model”

4.8. Create “dataset_root” in “classify_model” folder

4.9. Copy and paste those downloaded folders and Ipynb files from DECT into the classification folder under the root of the Nvidia clara-train-example folder. e.x. ~/clara-train-examples-master/classify_model.

![image](https://user-images.githubusercontent.com/31482058/114258988-ac50fe80-9998-11eb-8433-30b2c9f2d96a.png)

4.10. The pre-trained models are ready to be used in classification. The actual model files are in the “models” folder. The pre-trained model could be fine-tuning or re-train by the user's data. We re-trained the classification model with our own dataset consist of CT images from BIMCV, RSNA, MIDRC, and LIDC. The re-trained classification model was used in the DECT testing framework to evaluate the performance of enhancement AI.

## How to run
1. Replace the following lines from classification_AI.ipynb with the corresponding data directories. "nii_root" is the path of enhancement model folder. "MMAR_ROOT" is the path of classification model folder.

####################DATA DIRECTORY################### 

nii_root = '/claraDevDay/enhancement_model'

MMAR_ROOT="/claraDevDay/classify_model"

#####################################################

2. Run nvidia-docker:

2.1.
```bash
cd ~/NoteBooks/scripts
```

2.2.
```bash
sudo bash startDocker.sh 8890 '"device=1,3"' 5000
```

3. Open the JupyterLab link with the browser

4. Run classification_AI.ipynb or classification_AI.ipynb

## Output
1. classify_model/eval: positive and negative possibility scores (.csv)
