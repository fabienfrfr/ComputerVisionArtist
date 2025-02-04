# Animate step of understanding image

See the notebook [here](/notebook_computer-vision_ann.ipynb) (Work in progress)

Add : 
	- Les couches résiduelles sont l'une des solutions au probleme de l'effacement du gradient
 	- Les "têtes d'attentions" sont à la corrélation douce, ce que les couches convolutionnel sont à la convolution.
  	- Le critère de Markov


Image Captionning with Transformer - This model is based on this [paper](https://arxiv.org/abs/1502.03044).

###### Attribution required : Fabien Furfaro (CC 4.0 BY NC ND SA)


```bash
"A person is standing on top of a cliff"

How does an artificial neural network understand an image? This program is the result of a learning process that automatically annotates images caption and where each step of the process is represented in this animation. More details in my github notebook.

#deeplearning #coder #artificialintelligence #computerscience #machinelearning #ai #neuralnetwork #convolution #tech #attention #caption #pytorch #opencv
```


## Tips :

If you have a old nvidia driver GPU doesn't compatible with CUDA 10.2 (https://docs.nvidia.com/deploy/cuda-compatibility/index.html) and you want to use GPU calculation (like me), you need to do some requirement :

0 - OS compatibilities :
```bash
	- Nvidia doesnt maintain old version of CUDA in new Ubuntu version # for exemple use ubuntu 18.04 LTS for 390
```
Other, verify in https://developer.nvidia.com/cuda-gpus the compute capability of your gpu,  the minimum cuda capability that pytorch support is 3.5 (CUDA capability 3.0 support was dropped in v0.3.1). 

If you have an GPU with compute compatibilities < 3.5, you have 2 possibilities :

- 1) Install an very old PyTorch
- 2) Build PyTorch from source

See in bellow.

1 - Install the highest version driver :
```bash
	- sudo add-apt-repository ppa:graphics-drivers/ppa # update after
	- sudo ubuntu-drivers autoinstall # before : apt-get remove --purge nvidia-*
	- ubuntu-drivers devices # see the last version
	- sudo apt install nvidia-driver-XXX-XXX # for me : 418-server
	- sudo apt install nvidia-utils-XXX-XXX # idem
	#- sudo apt-get install nvidia-modprobe
```
2 - Intall toolkit (following ubuntu version compatibilities : https://developer.nvidia.com/cuda-toolkit-archive) :
```bash
	- sudo apt install nvidia-cuda-toolkit
	(OR)
	- wget https://developer.nvidia.com/compute/cuda/10.1/Prod/local_installers/cuda-repo-ubuntu1804-10-1-local-10.1.105-418.39_1.0-1_amd64.deb # ubuntu 18.04 but compatible in 20.04
	- sudo apt list --installed | grep cuda-repo-ubuntu1804-10-1-local-10.1.105-418.39
	(OR)
	- wget https://developer.nvidia.com/compute/cuda/10.1/Prod/local_installers/cuda_10.1.168_418.67_linux.run
	
	#- https://developer.nvidia.com/compute/cuda/9.1/Prod/local_installers/cuda_9.1.85_387.26_linux
```

3 - Testing :
```bash
	- nvidia-smi # if doesn't works, reinstall driver and doesn't install toolkit
	- nvcc --version # if toolkit it's installed
```

Cuda 9.1 & drivers 390 in Ubuntu 18.04 (Python 3.6):

	- sudo apt-get install libjpeg-dev zlib1g-dev
	- sudo apt-get install openjdk-8-jdk git python-dev python3-dev python-numpy python3-numpy python-six python3-six build-essential python-pip python3-pip python-virtualenv swig python-wheel python3-wheel libcurl3-dev libcupti-dev

	- https://developer.nvidia.com/cudnn (need to register to install cuDNN)
		- libcudnn7 (deb file download)

	- pip3 install pillow==6.2.0 scikit-build cmake
	- pip3 install torch==1.1.0 torchvision==0.3.0 -f https://download.pytorch.org/whl/cu90/torch_stable.html
	- (in Python3) torch.cuda.is_available()

4 - Install adapted python version (for old pytorch, it's python 3.8 max!) :
```bash
	- sudo apt install software-properties-common -y
	- sudo add-apt-repository ppa:deadsnakes/ppa -y
		- sudo add-apt-repository -y ppa:jblgf0/python #for Ubuntu < 16.10
	- sudo apt install python3.8 -y
	- sudo apt install python3.8-dev python3.8-venv python3.8-distutils python3.8-lib2to3 python3.8-gdbm python3.8-tk -y
		- sudo apt install python3.5-dev python3.5-venv python3.5-gdbm python3.5-tk -y
```
5 - Install python package in 3.8 specific version :
```bash
		- python3.6 -m ensurepip --default-pip --user
		- python3.6 -m pip install ipython --user
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

## Old PyTorch


1 - Install an very old PyTorch

You install pytorch with source (whl file : https://pytorch.org/get-started/previous-versions/), minimum is 0.3.0, for exemple, is "cu75/torch-0.3.0.post4-cp36-cp36m-linux_x86_64.whl", if your python is 3.6. Download, and run command (order is important) :

	- python3.5 -m pip install torchvision==0.2.1 # adapted for cuda9
	- python3.5 -m pip uninstall torch
	- pip install torch-0.3.0.post4-cp35-cp35m-linux_x86_64.whl

But.. you need to add many function changement (name, etc.) if you want to use my code, like that (for exemple, the very basic function "tensor", is "Tensor" in very old version) :

```bash
def rename(newname):
    def decorator(f):
        f.__name__ = newname
        return f
    return decorator
```
And then use it like this:
```bash
@rename('new name')
def f():
    pass
print f.__name__
```
Creation of "utils_oldpytorch" in progress.. not recommended now ! Or install python specific version for test !

	- pip install --upgrade pip setuptools wheel #not for old version (< 18.04)
	- pip install scikit-build cmake

Many problem for 3.5, if error message of setuptools, delete python3.5 folder in ".local/lib", remove python3-pip and reinstall.

2 - Build from source (highly recommanded, but long way : ~ 6h)

My spec :

- Nvidia Quadro K1100M (Cuda compute capability 3.0)
- Ubuntu 20.04 (driver-418-server + cuda10.1 + python3.8 per default) - recommanded
- Cudnn8 (Installed by libcudnn8_8.0.5.39-1+cuda10.1_amd64.deb : normally adapted for 18.04, but it's works -- Download in : https://developer.nvidia.com/rdp/cudnn-archive, need registration, or tips, links in : https://ubuntu.pkgs.org/18.04/cuda-amd64/libcudnn8_8.0.5.39-1+cuda10.1_amd64.deb.html)


If you are a same spec (very good luck for you), I can send the wheel files build to install by pip. (I have also a build for 18.04, cu91, cp35 and compute capability 3.0).

To build from source, you need to see precisely your cuda, gcc, python, cudnn version and after you can build following the good realese. When you build yourself, pytorch go more faster.

See following the good branch, and the procedure in pytorch readme :

```bash
sudo apt install cmake
pip3 install numpy ninja pyyaml mkl mkl-include setuptools cmake cffi typing_extensions future six requests dataclasses
git clone --recursive https://github.com/pytorch/pytorch # see specific branch checkout (OR :)
	# git clone -b v1.0.1 --recursive https://github.com/pytorch/pytorch 
	# git clone --depth 1 -b v1.1.0 --recursive https://github.com/pytorch/pytorch
	# git clone --depth 1 -b base/1.5 --recursive https://github.com/pytorch/pytorch
	# git clone --depth 1 -b lts/release/1.8 --recursive https://github.com/pytorch/pytorch (recommanded --> Cuda 9 compatibility)
```

(Advice) You need to switch between multiple gcc compiler :

	- sudo apt install build-essential gcc software-properties-common
	- sudo add-apt-repository ppa:ubuntu-toolchain-r/test -y
		- sudo gedit /etc/apt/sources.list
		- deb http://fr.archive.ubuntu.com/ubuntu/ bionic main universe (add to file, fr=france) 
	- sudo apt-get install gcc-7 g++-7 -y
	- sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 7
		- sudo ln -s /usr/bin/gcc-7 /usr/bin/gcc # default
		- sudo ln -s /usr/bin/g++-7 /usr/bin/g++ # default
		- sudo ln -s /usr/bin/gcc-7 /usr/bin/cc # default
		- sudo ln -s /usr/bin/g++-7 /usr/bin/c++ # default
	- sudo update-alternatives --config gcc # gcc --version

Basicaly the command sequence is :

```bash
cd ~/pytorch
git submodule update --init
export CUDAHOSTCXX='/usr/bin/gcc-7'  # or make default, 5 for cuda 9.1 (AND) export CUDA_HOST_COMPILER=/usr/bin/gcc-5 / export DCUDA_HOST_COMPILER=/usr/bin/gcc-5
	- (see) cmake -DCMAKE_C_COMPILER=gcc-6 -DCMAKE_CXX_COMPILER=g++-6
export CMAKE_CXX_COMPILER=g++-7 # or make default, 5 for cuda 9.1
export TORCH_CUDA_ARCH_LIST=3.0 # or =all
python3 setup.py install > build.log
# if you want to share, rather use : 
python3 setup.py bdist_wheel > build.log
```

Error (not with bdist_wheel) :
	- error: could not create '/usr/lib/python3.8/site-packages': Permission denied (USE) sudo ?
	- "torch/C extension rather than C extension" (USE) python3 setup.py develop && python -c "import torch"
	- torch.C.cuda_getDeviceCount() > 0 : incompatible driver-cuda (USE) other driver.


(Advice) Use virtual environment for installation test (install requirement also) :

```bash
python3 -m venv $HOME/venv
source $HOME/venv/bin/activate
which python
which pip
python --version
pip --version
# if you want test before install
./configure --help | less # (q) to quit / for classic install, not include in pytorch
make
python3 setup.py clean
```


Also, you can build with docker for python3.5 by (read and follow instruction in Dockefile) :

	- sudo docker build -t build-torch .
	- sudo docker save build-torch > torch_image.tar

Find wheel file in the image. This part is 99% based on https://github.com/edumucelli/build-pytorch.

Download torchvision-0.2.0-py2.py3-none-any.whl in https://download.pytorch.org/whl/cu90/torch_stable.html, and install it with :

	- python3.5 -m pip install torchvision-0.2.0-py2.py3-none-any.whl 
	- python3.5 -m pip install torch-1.0.0a0+db5d313-cp35-cp35m-linux_x86_64.whl (after install torchvision necessary.. stupid, but dont change name of file !!)
 

