# Animate step of understanding image

See the notebook [here](/notebook_computer-vision_ann.ipynb) (Work in progress)

Image Captionning with Transformer - This model is based on this [paper](https://arxiv.org/abs/1502.03044).

###### Attribution required : Fabien Furfaro (CC 4.0 BY NC ND SA)


```bash
"A person is standing on top of a cliff"

How does an artificial neural network understand an image? This program is the result of a learning process that automatically annotates images caption and where each step of the process is represented in this animation. More details in my github notebook.

#deeplearning #coder #artificialintelligence #computerscience #machinelearning #ai #neuralnetwork #convolution #tech #attention #caption #pytorch #opencv
```


## Tips :

If you have a old nvidia GPU doesn't compatible with CUDA 10.2 and you want to use GPU calculation (like me), you need to do some requirement :

0 - OS compatibilities :
```bash
	- Nvidia doesnt maintain old version of CUDA in new Ubuntu version # for exemple use ubuntu 18.04 LTS for 418-server
```

1 - Install the highest version driver :
```bash
	- sudo ubuntu-drivers autoinstall
	- ubuntu-drivers devices # see the last version
	- sudo apt install nvidia-driver-XXX-XXX # for me : 418-server
	- sudo apt install nvidia-utils-XXX-XXX # idem
	#- sudo apt-get install nvidia-modprobe
```
2 - Intall toolkit (following ubuntu version compatibilities) :
```bash
	- sudo apt install nvidia-cuda-toolkit
	(OR)
	- wget https://developer.nvidia.com/compute/cuda/10.1/Prod/local_installers/cuda-repo-ubuntu1804-10-1-local-10.1.105-418.39_1.0-1_amd64.deb # ubuntu 18.04 but compatible in 20.04
	- sudo apt list --installed | grep cuda-repo-ubuntu1804-10-1-local-10.1.105-418.39
	(OR)
	- wget https://developer.nvidia.com/compute/cuda/10.1/Prod/local_installers/cuda_10.1.168_418.67_linux.run
```

3 - Testing :
```bash
	- nvidia-smi # if doesn't works, reinstall driver and doesn't install toolkit
	- nvcc --version # if toolkit it's installed (not necessary)
```
4 - Install adapted python version (for old pytorch, it's python 3.8) :
```bash
	- sudo apt install software-properties-common -y
	- sudo add-apt-repository ppa:deadsnakes/ppa -y
	- sudo apt install python3.8 -y
	- sudo apt install python3.8-dev python3.8-venv python3.8-distutils python3.8-lib2to3 python3.8-gdbm python3.8-tk -y
```
5 - Install python package in 3.8 specific version :
```bash
	- python3.8 -m pip install PACKAGES==VERSION
	- python3.8 -m pip install torch==1.8.1+cu101 torchvision==0.9.1+cu101 torchaudio==0.8.1 -f https://download.pytorch.org/whl/torch_stable.html --use-deprecated=html5lib
	- python3.8 -m pip install ipython
	- python3.8 -m pip install matplotlib --force-reinstall
	- python3.8 -m pip install opencv-python
```

6 - Use specific python version :
```bash
	- python3.8 files.py --arg
	(OR)
	- python3.8 -m IPython
		- run files.py --arg
```


ELSE (exemple) :
```bash
	- specify calculation per CPU in pytorch : torch.load(PATH, map_location=torch.device('cpu'))
	- OR generalize : torch.load(PATH, map_location=torch.device('cuda:0' if torch.cuda.is_available() else 'cpu')
	- doesnt use command like "tensor.gpu()" directly ! use conditional statement
```