# -*- mode: ruby -*-
# vi: set ft=ruby :

servers = {
  'master-01' => { 'ip' => '10.10.10.10', 'memory' => '2048', 'cpus' => '2', 'disk' => '/tmp/tpxBQAzZif8c4berq75K.vdi' },
  'worker-01' => { 'ip' => '10.10.10.11', 'memory' => '2048', 'cpus' => '2', 'disk' => '/tmp/6KzmetxXetd529zIFFZ4.vdi' },
  'worker-02' => { 'ip' => '10.10.10.12', 'memory' => '2048', 'cpus' => '2', 'disk' => '/tmp/tw3RbtE0eb2kCHp1CbfL.vdi' }
}

Vagrant.configure("2") do |config|

  servers.each do |name, conf|
    config.vm.define "#{name}" do |cfg|
      cfg.vm.box = "ewerton_silva00/ubuntu-minimal-20.04-amd64"
      cfg.vm.box_version = "20220101.0"
      cfg.vm.box_check_update = true
      cfg.vm.hostname = "#{name}.#{conf['ip']}.nip.io"
      cfg.vm.network "private_network", ip: "#{conf['ip']}"
      cfg.vm.provision :hosts, :sync_hosts => true

      cfg.vbguest.auto_update = false

      cfg.vm.provider "virtualbox" do |vb|
        vb.name = "#{name}"
        vb.gui = false
        vb.cpus = "#{conf['cpus']}"
        vb.memory = "#{conf['memory']}"
        # --- Add second disk with 10GB. Define the variable named 'disk'. e.g. 'disk' => '/tmp/tpxBQAzZif8c4berq75K.vdi'
        vb.customize ['createhd', '--filename', "#{conf['disk']}", '--size', 10 * 1024]
        vb.customize ['storageattach', :id, '--storagectl', 'SATA', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', "#{conf['disk']}"]
      end

      cfg.vm.provision "shell", inline: <<-SHELL
        # Configure second disk
        sudo parted --script --align optimal /dev/sdb mklabel gpt -- mkpart primary ext4 0% 100%
        sudo mkfs --type ext4 /dev/sdb1
        sudo mkdir /mnt/data
        sudo mount --types ext4 /dev/sdb1 /mnt/data
        sudo su -c "echo '/dev/sdb1 /mnt/data ext4 defaults 0 0' >> /etc/fstab"
        # Update all packages to their latest version
        sudo apt update && sudo apt upgrade -y
        hostnamectl
      SHELL
    end
  end
end
