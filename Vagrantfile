# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "bento/ubuntu-18.04"

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--memory", "2048"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
    vb.cpus = 2
  end

  config.vm.define "kubemaster" do |kubemaster|
    kubemaster.vm.hostname = "kubemaster"
    kubemaster.vm.network "private_network", ip: "172.20.27.10"
  end

  config.vm.define "kubenode01" do |kubenode01|
    kubenode01.vm.hostname = "kubenode01"
    kubenode01.vm.network "private_network", ip: "172.20.27.11"
  end

  config.vm.define "kubenode02" do |kubenode02|
    kubenode02.vm.hostname = "kubenode02"
    kubenode02.vm.network "private_network", ip: "172.20.27.12"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "generate_inventory.yml"
    ansible.limit = "all"
    ansible.groups = {
      "cl_kubevagrant" => ["kubemaster","kubenode01", "kubenode02"],
      "tag_kubemaster" => ["kubemaster"],
      "tag_kubenode01" => ["kubenode01"],
      "tag_kubenode02" => ["kubenode02"],
      "cl_kubeworkers" => ["kubenode01", "kubenode02"]
    }

    ansible.host_vars = {
      "kubemaster" => {"kubemaster" => "172.20.27.10", "tag" => "kubemaster"},
      "kubenode01" => {"kubenode01" => "172.20.27.11", "tag" => "kubenode01"},
      "kubenode02" => {"kubenode02" => "172.20.27.12", "tag" => "kubenode02"}
    }

  end

end
