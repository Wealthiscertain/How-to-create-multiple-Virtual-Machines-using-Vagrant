# How-to-create-multiple-Virtual-Machines-using-Vagrant

![image](https://github.com/user-attachments/assets/c36ec11c-f30f-4159-b677-3934d3fe4008)

### **Tools used**
- **Vagrant:** Automates the creation and management of virtual environments.
- **CentOS:** A free, enterprise-class Linux distribution, ideal for server environments.
- **Git Bash:** A command-line tool that allows users to run Git commands and other Unix-based utilities on Windows.
- **VsCode:** A source-code editor for development
- **Oracle VM Virtual Box Manager:** Virtualization software for running multiple virtual machines.

If you're working on a project that requires multiple virtual machines (VMs) to run different services (e.g., a database on one VM, the application service on another, and the frontend on a separate VM), Vagrant makes managing this setup much easier. By using a Multi-VM Vagrant file, you can define and run multiple VMs from a single configuration file. This guide will show you how to create and configure multiple VMs using Vagrant.

### **Step-by-step setup**

**1. Check the Vagrant Documentation:**
First, head to the Vagrant documentation and look for the section on Multi-machine. This section provides a useful template for defining multiple machines in a single Vagrantfile.

**2. Understand the Basic Multi-VM Template:**
The basic idea is to define each machine using the config.vm.define method. This creates separate configurations for each VM within the same Vagrantfile. Here's a simple example from the Vagrant documentation:

```
Vagrant.configure("2") do |config|
   config.vm.provision "shell", inline: "echo Hello"
  config.vm.define "web" do |web|
  web.vm.box = "apache"
  end
  config.vm.define "db" do |db|
  db.vm.box = "mysql"
  end
end
```

**3. Create a Folder for Your Project:**
Create a folder called `multivm` where you'll store your Vagrantfile. This is where all your VMs will be defined and managed.

**4. Edit and Customize the Vagrantfile:**
Now let's create a custom Vagrantfile with multiple VMs. In this example, we will define two web servers (`web01`, `web02`, `web03`) and one database server (`db01`). Each machine will be assigned its own private IP address.

```
Vagrant.configure("2") do |config|
    config.vm.define "web01" do |web01|
      web01.vm.box = "ubuntu/focal64"
      web01.vm.hostname = "web01"
      web01.vm.network "private_network", ip: "192.168.56.41"
    end
  
    config.vm.define "web02" do |web02|
      web02.vm.box = "ubuntu/focal64"
      web02.vm.hostname = "web02"
      web02.vm.network "private_network", ip: "192.168.56.42"
    end
  
    config.vm.define "web03" do |web03|
        web03.vm.box = "ubuntu/focal64"
        web03.vm.hostname = "web02"
        web03.vm.network "private_network", ip: "192.168.56.43"
      end

    config.vm.define "db01" do |db01|
      db01.vm.box = "centos/7"
      db01.vm.hostname = "db01"
      db01.vm.network "private_network", ip: "192.168.56.44"
      db01.vm.provision "shell", inline: <<-SHELL
        yum install -y wget unzip mariadb-server -y
        systemctl start mariadb
        systemctl enable mariadb
        # Additional provisioning steps for db01
      SHELL
    end
 end
```

This Vagrantfile defines:
- Three web servers (web01, web02, web03) running on Ubuntu 20.04.
- One database server (db01) running on CentOS 7 with MariaDB installed.

5. Open **Git Bash** or your preferred terminal and navigate to the `multivm` folder where you saved your Vagrantfile.
   
Now, to start all the VMs, run `vagrant up`

Vagrant will take care of setting up and starting all the VMs as defined in the Vagrantfile. Each VM will have its private IP address and hostname, making it easy to network them together.

![image](https://github.com/user-attachments/assets/39e606fd-25fb-4829-bb2d-cb1a5fd9bdde)

![image](https://github.com/user-attachments/assets/4563d4bc-9ff9-4a86-ad78-5d3f03be29b0)

![image](https://github.com/user-attachments/assets/c6fdcd8b-b437-4204-a5e2-efb64ce919f4)

![image](https://github.com/user-attachments/assets/b361e29d-7f9d-47de-a097-717abcaad2e1)

**Key Takeaways:**

- **Multiple VMs:** You can define multiple virtual machines in a single Vagrantfile, which is useful when you need to run different services in separate environments.

- **Network Configuration:** Each VM can have its own private IP address, making it easy to network the machines together.
  
- **Automation:** Vagrant's provisioning scripts allow you to automate software installation and configuration, like setting up a database or a web server.
