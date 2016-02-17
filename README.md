# deep-learning-setup

Easy instructions for setting up a deep learning environment to use the GPU.
Written for software engineers and researchers who have experience with linux
but are new to configuring their box for deep learning.

Tested on Ubuntu 14.04 but should work with minimal tweaks on similar systems.

Note: the guide currently only uses Theano since Google's TensorFlow uses an
older version of CUDA.

# background

Whether setting up my local box or a GPU-enabled machine on AWS, I often ran
into issues getting Theano to use the GPU correctly. This guide should be a
one-stop shop for setting up your GPU to work with Theano and get started doing
some deep learning!

# coming soon

The instructions below will be converted to a bash script so you can easily run
without needing to copy paste so much.

# assumptions

I'm assuming you know how to use the command line. I'm also assuming you have
Python installed along with the Theano package. If you don't have Python
installed, check out the excellent [Anaconda](https://www.continuum.io/downloads)
distribution.

# step 1: verify

Let's make sure we're up to date:

```bash
sudo apt-get update && sudo apt-get upgrade
```

Let's make sure we have an nvidia GPU detected:

```bash
lspci | grep -i nvidia
```

# step 2: download CUDA drivers

Go [here](https://developer.nvidia.com/cuda-downloads) to download the
appropriate CUDA drivers for your system. I use the deb (local) option.

# step 3: unpack and install the CUDA drivers

You may need to update the name of the .deb as nvidia releases updates.

```bash
cd ~/Downloads
sudo dpkg -i cuda-repo-ubuntu1404-7-5-local_7.5-18_amd64.deb
sudo apt-get update && sudo apt-get install cuda
```

# step 4: configure theano settings

Check out the guide [here](http://deeplearning.net/software/theano/library/config.html)
Feel free to tweak these:

```bash
cd
echo -e "\n[global]\nfloatX=float32\ndevice=gpu\n[mode]=FAST_RUN\n\n[nvcc]\nfast
math=True\n\n[cuda]\nroot=/usr/local/cuda" >> ~/.theanorc
```

# step 5: configure the path

Note: this assumes you installed CUDA using the defaults. AKA you didn't change
anything during the installation above.

```bash
cd
echo -e "\nexport PATH=/usr/local/cuda/bin:$PATH\n\nexport LD_LIBRARY_PATH=/usr/
local/cuda/lib64" >> .bashrc
```

# step 6: test your GPU

Go [here](http://deeplearning.net/software/theano/tutorial/using_gpu.html) and
run the script.

# common issues

Theano will typically require sudo permission the first time it's invoked. You
could create an alias, but please watch out when using sudo:

```bash
cd
echo "alias supy='sudo /home/mason/anaconda2/bin/python'" >> ~/.bash_aliases
```

You might need to refresh the shared library cache with CUDA:

```bash
sudo ldconfig /usr/local/cuda/lib64
```

The above 2 common issues will typically solve the most common issues when
trying to get the GPU working. 

# contributing

Please feel free to file an issue or make a pull request if you have anything
to add to make this guide better or easier to use!
