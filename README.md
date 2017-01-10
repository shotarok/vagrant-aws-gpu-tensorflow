vagrant-aws-gpu-tensorflow
====

A Vagrantfile to provision a gpu instance installed tensorflow-gpu on EC2

## Description

This Vagrantfile gives you an easy way to launch a gpu instance installed tensorflow-gpu. Information for the instance which will be launched is blow.

* CUDA: v8.0
* cuDNN: v5.1
* Python: 3.4
* Tensorflow(GPU): v12.1
* OS: Ubuntu 14.04
* instance_type: g2.2xlarge
* region: np-northeast-1
* ami: ami-3995e55e

If you want to change the *region*, please find an appropriate *ami* for your region and instance_type from [here](https://cloud-images.ubuntu.com/locator/ec2/). If you want to change the version of *python*, *CUDA* or *cuDNN*, please find an appropriate *tensorflow-gpu* from [here](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/g3doc/get_started/os_setup.md).

## Requirement

Vagrant and a few configurations of AWS are required.

### Vagrant

[Vagrant](https://www.vagrantup.com/) is a command line utility to manage virtual machines. You can download an appropriate installer for your platform from [here](https://www.vagrantup.com/downloads.html).

[vagrant-aws](https://github.com/mitchellh/vagrant-aws) is a vagrant plugin to allow Vagrant to control and provision machines in EC2 and VPC. When Vagrant is already installed, you can install it like blow.

```
$ vagrant plugin install vagrant-aws
```

### AWS Configuration

 Amazon EC2 is a VPS service. This Vagrantfile needs network and security configurations, which are [Amazon EC2 Key Pairs](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) and  [Amazon EC2 Security Groups](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html).


 You can create a key pairs accoding to [this link](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair). You can also create a security group according to [this link](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html#creating-security-group).

 After creating them, **please set them to `aws.keypair_name`, `aes.security_groups` and `ssh.private_key_path` in the Vagrantfile**, whose roles are explained at [vagrant-aws's README](https://github.com/mitchellh/vagrant-aws#configuration). **Please use a security group to allow SSH access.** If you want to use [jupyter notebook](http://jupyter.org/) and [tensorboard](https://www.tensorflow.org/how_tos/summaries_and_tensorboard/), I recommend you to allow TCP 8888 and TCP 6006 ports.

## Usage

```shell
$ git clone https://github.com/shotarok/vagrant-aws-gpu-tensorflow.git
$ cd vagrant-aws-gpu-tensorflow

# Configure aws environment variables
$ export AWS_ACCESS_KEY_ID=...
$ export AWS_SECRET_ACCESS_KEY=...

# Set your own aws.keypair_name, aws.security_groups and
# ssh.private_key_path by your favorite editor
$ vi Vagrantfile

# Launch and provision an gpu instance
# (it will take some time)
$ VAGRANT_LOG=info vagrant up --provider=aws

# Connect to the instance
$ vagrant ssh

# Check tensorflow-gpu on the gpu instance
(guest)$ python3 -c "import tensorflow"
> I tensorflow/stream_executor/dso_loader.cc:128] successfully opened CUDA library libcublas.so locally
> I tensorflow/stream_executor/dso_loader.cc:128] successfully opened CUDA library libcudnn.so locally
> I tensorflow/stream_executor/dso_loader.cc:128] successfully opened CUDA library libcufft.so locally
> I tensorflow/stream_executor/dso_loader.cc:128] successfully opened CUDA library libcuda.so.1 locally
> I tensorflow/stream_executor/dso_loader.cc:128] successfully opened CUDA library libcurand.so locally
> ...

# Terminate the instance
$ vagrant destroy
```

## Contribution

1. Fork
2. Create a feature branch
3. Commit your changes
4. Rebase your local changes against the master branch
5. Create new Pull Request

## Licence

[MIT](https://github.com/shotarok/vagrant-aws-gpu-tensorflow/blob/master/LICENCE)

## Author

[Shotaro Kohama](https://github.com/shotarok)
