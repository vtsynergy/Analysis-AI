# Analysis AI

The modelS in this folder is used to segment the lung region from the 3D chest CT images and classify the 3D chest CT images to COVID-19 positive or negative case.

The seg_model contains a script and folder structure used to run segmentation operation when the segmentation model has been set.

The classify_model contains a script and folder structure used to run classification operation when the classification model has been set.

## Platform

We tested the framework on platform shown below:

Operating System: Linux

Architecture: x86_64

Distribution: Ubuntu

Version: 18.04

## Hardware requirements
One or more Nvidia GPUs. (Two or more GPUS are recommended for advanced analysis features.) This GPU version is only compatible with Nvidia GPUs having compute capability 6.0 or higher, i.e., any GPU from Pascal, Volta, Turing, Ampere, and beyond will work.

## Software requirements

Nvidia-docker and Nvidia Clara SDK are required to run the package. If you already have docker installed then user should be in the docker group. Otherwise, sudo access is needed to install the prerequisite. Nvidia Clara SDK requires Nvidia CUDA 11.0.194 and Nvidia Driver release 450 or later. It requires Clara Train 3.1 with TensorFlow (Clara Train 4.0 with PyTorch is not supported now).

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

The pre-trained segmentation and classification model could be download from NVIDIA NGC:

4.1. Go to the homepage of NVIDIA NGC [here](https://ngc.nvidia.com/signin) and Log in with an NVIDIA account. If you do not have an NVIDIA account, sign up first.

![image](https://user-images.githubusercontent.com/31482058/114258934-46fd0d80-9998-11eb-84ff-8dc16dca538c.png)

4.2. Choose “MODELS” at the top of the homepage, and click it.

![image](https://user-images.githubusercontent.com/31482058/114258951-5aa87400-9998-11eb-8b7a-82088720076c.png)

4.3. Type in “covid” and search.

![image](https://user-images.githubusercontent.com/31482058/114258957-6bf18080-9998-11eb-8828-e4a27c2ff34a.png)

4.4. Click “Download” to download the "clara_train_covid19_ct_lung_seg" or "clara_train_covid19_3d_ct_classification" package. The example in following graph is "clara_train_covid19_ct_lung_seg" package.

![image](https://user-images.githubusercontent.com/31482058/114258966-7ca1f680-9998-11eb-8b5d-3e7dce27ac9e.png)

4.5. Extract the downloaded compressed file, and get several folders

![image](https://user-images.githubusercontent.com/31482058/114258972-87f52200-9998-11eb-90a0-cc597f7b033e.png)

4.6. Put the “enhancement_model” folder downloaded from DECT into this path: ~/clara-train-examples-master/enhancement_model

4.7. Create two folders under the root of Nvidia clara-train-example folder: “seg_model” and “classify_model”

4.8. Create “dataset_root” in “seg_model” folder and “classify_model” folder.

4.9. Copy and paste those downloaded folders and Ipynb files from Analyze-AI repository into the segmentation and classification folders under the root of the Nvidia clara-train-example folder. e.x. ~/clara-train-examples-master/seg_model.

![image](https://user-images.githubusercontent.com/31482058/114258988-ac50fe80-9998-11eb-8433-30b2c9f2d96a.png)

4.10. The pre-trained models are ready to be used in segmentation and classification. The actual model files are in the “models” folder. The pre-trained model could be fine-tuning or re-train by the user's data. We re-trained the classification model with our own dataset consist of CT images from BIMCV, RSNA, MIDRC, and LIDC. The re-trained classification model was used in the DECT testing framework to evaluate the performance of enhancement model.

## How to run
1. Replace the following lines from segmentaion_AI.ipynb (or classification_AI.ipynb) with the corresponding data directories. 

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

4. Run segmentation_AI.ipynb (or classification_AI.ipynb)

## Run as a non-privileged user in a multi-user environment
1. Add user to the docker group
2. Add the following to the run line in startDocker.sh
```
  --user $(id -u):$(id -g) \
  -v ${HOME}/.local/:/.local/ \
  -v ${HOME}/.config/:/.config/ \
```
 Making sure that the .local and .config directories exist in the user's home directory on the host system
 
3. When calling the./startDocker.sh script pay attention to whether any of the default ports are already in use, if so you can override them on the command line. (port for notebook and AIAA_PORT). You do not need to make any changes to the port bindings on the docker run line, the script remaps them itself.

4. Allow jupyter lab to listen to traffic originating from machines other than localhost. Add the --ip=0.0.0.0 flag in the startJupyter script (Recommended to only use behind a firewall)

5. (Option) If behind a firewall: Use a SOCKS proxy to forward HTTP traffic over SSH so you can access the Jupyter lab HTTP interface without being physically on the system. 

## Output
1. seg_model/eval: segmentation masks (.nii)
2. classify_model/eval: positive and negative possibility scores (.csv)
