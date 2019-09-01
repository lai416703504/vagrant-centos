# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  (1..3).each do |i|
    
    config.vm.define "node#{i}" do |node|
    
    # 设置虚拟机的box
    config.vm.box = "CentOS-7.box"

    # 设置虚拟机的主机名
    node.vm.hostname = "node#{i}"

    # 设置虚拟机IP
    node.vm.network "private_network", ip: "192.168.66.#{i}"

    # 设置主机与虚拟机的共享目录
    node.vm.synced_folder "D:\\vagrant_dir",  "/home/vagrant/share", create:true, owner: 'vagrant', :mount_options => ["dmode=777","fmode=777"]

    # VirtualBox相关配置
    node.vm.provider "virtualbox" do |v|

      # 设置虚拟机的名称
      v.name = "node#{i}"

      # 设置虚拟机的内存大小
      v.memory = 2048

      # 设置虚拟机的cpu个数
      v.cpus = 1
    end

    # 使用shell脚本进行软件安装和配置
    node.vm.provision "shell" ,inline: <<-SHELL

     yum update
     yum install -y wget
      # 修改源
      mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

      wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS7-Base-163.repo

      yum clean all
      yum makecache

      # 安装docker
      sudo yum remove docker docker-common docker-selinux docker-engine

      sudo yum install -y yum-utils device-mapper-persistent-data lvm2

      wget -O /etc/yum.repos.d/docker-ce.repo https://mirrors.ustc.edu.cn/docker-ce/linux/centos/docker-ce.repo

      sudo sed -i 's+download.docker.com+mirrors.tuna.tsinghua.edu.cn/docker-ce+' /etc/yum.repos.d/docker-ce.repo

      sudo yum makecache fast
      sudo yum install docker-ce
    SHELL
    end

  end

end
