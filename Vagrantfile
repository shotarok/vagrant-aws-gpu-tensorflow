# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "dummy"
  config.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"

  # work around for vagrant-aws v0.7.2 and vagrant v1.9.1.
  # Please see https://github.com/mitchellh/vagrant/issues/8118
  config.ssh.pty = false

  config.vm.provider :aws do |aws, override|
    aws.access_key_id = ENV['AWS_ACCESS_KEY_ID']
    aws.secret_access_key = ENV['AWS_SECRET_ACCESS_KEY']

    # If you want to use other region,
    # please find an appropriate public ami from
    # https://cloud-images.ubuntu.com/locator/ec2/
    aws.region = "ap-northeast-1"
    aws.ami = "ami-3995e55e"
    aws.instance_type="g2.2xlarge"

    aws.keypair_name = "KEYPAIR NAME"
    aws.security_groups = "SECURITY GROUP NAME"

    override.ssh.username = "ubuntu"
    override.ssh.private_key_path = "PATH TO YOUR PRIVATE KEY"
  end

  config.vm.provision "shell", inline: <<SCRIPT
apt-get update && apt-get -y upgrade
apt-get -y install linux-headers-$(uname -r) linux-image-extra-$(uname -r)

# Install CUDA v8.0
dpkg --configure -a
wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1404/x86_64/cuda-repo-ubuntu1404_8.0.44-1_amd64.deb
dpkg -i cuda-repo-ubuntu1404_8.0.44-1_amd64.deb
apt-get update && apt-get install -y cuda-8-0
rm cuda-repo-ubuntu1404_8.0.44-1_amd64.deb

# Install cuDNN v5.1
wget http://developer.download.nvidia.com/compute/redist/cudnn/v5.1/cudnn-8.0-linux-x64-v5.1.tgz
tar xvzf cudnn-8.0-linux-x64-v5.1.tgz*
rm cudnn-8.0-linux-x64-v5.1.tgz
cp cuda/include/cudnn.h /usr/local/cuda/include/
cp cuda/lib64/libcudnn* /usr/local/cuda/lib64/
chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
rm -rf /home/ubuntu/cuda

# Setup library paths
echo 'export CUDA_HOME=/usr/local/cuda
export CUDA_ROOT=/usr/local/cuda
export PATH=\$PATH:\$CUDA_ROOT/bin:$HOME/bin
export LD_LIBRARY_PATH=\$LD_LIBRARY_PATH:\$CUDA_ROOT/lib64
' >> /home/ubuntu/.bashrc

# Install tensorflow for gpu
# Please see https://github.com/tensorflow/tensorflow/blob/master/tensorflow/g3doc/get_started/os_setup.md
curl https://bootstrap.pypa.io/get-pip.py | sudo python3
pip3 install --no-cache-dir https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-0.12.1-cp34-cp34m-linux_x86_64.whl
SCRIPT

end
