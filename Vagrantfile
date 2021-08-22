################################################################################
#
# Vagrantfile
#
################################################################################

### Change here for more memory/cores/disk size ###
VM_MEMORY=4096
VM_CORES=4
VM_DISK_SIZE='51200MB'

# Version declaration
RELEASE='v1.0'
VM_NAME="Netico Debian-var #{RELEASE}"

Vagrant.configure('2') do |config|
	config.vm.box = 'ubuntu/bionic64'
	config.disksize.size = VM_DISK_SIZE

	config.vm.provider :vmware_fusion do |v, override|
		v.vmx['memsize'] = VM_MEMORY
		v.vmx['numvcpus'] = VM_CORES
	end

	config.vm.provider :virtualbox do |v, override|
		v.memory = VM_MEMORY
		v.cpus = VM_CORES

		required_plugins = %w( vagrant-vbguest vagrant-disksize )
		required_plugins.each do |plugin|
		  system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
		end
	end

	config.vm.provision 'shell' do |s|
		s.inline = 'echo Setting up machine name'

		config.vm.provider :vmware_fusion do |v, override|
			v.vmx['displayname'] = VM_NAME
		end

		config.vm.provider :virtualbox do |v, override|
			v.name = VM_NAME
		end
	end

	config.vm.provision 'shell', privileged: true, inline:
		"sed -i 's|deb http://us.archive.ubuntu.com/ubuntu/|deb mirror://mirrors.ubuntu.com/mirrors.txt|g' /etc/apt/sources.list
		apt-get -q update
		apt-get -y upgrade
		apt-get purge -q -y snapd lxcfs lxd ubuntu-core-launcher snap-confine
		apt-get -q -y install build-essential libncurses5-dev \
            git tig vim bzr cvs mercurial subversion unzip bc \
            binfmt-support qemu qemu-user-static debootstrap kpartx \
            lvm2 dosfstools gpart binutils bison git lib32ncurses5-dev \
            libssl-dev python-m2crypto gawk wget git-core diffstat unzip \
            texinfo gcc-multilib build-essential chrpath socat libsdl1.2-dev \
            autoconf libtool libglib2.0-dev libarchive-dev python-git xterm \
            sed cvs subversion kmod coreutils texi2html bc docbook-utils \
            python-pysqlite2 help2man make gcc g++ desktop-file-utils \
            libgl1-mesa-dev libglu1-mesa-dev mercurial automake groff curl \
            lzop asciidoc u-boot-tools mtd-utils device-tree-compiler flex \
            debootstrap qemu-user-static qemu-system-arm
		apt-get -q -y autoremove
		apt-get -q -y clean
		update-locale LC_ALL=C"

	config.vm.provision 'shell', privileged: false, inline:
		"echo 'Downloading #{VM_NAME} repositories. This may take a while...'
        git clone https://github.com/netico-solutions/debian-var.git -b netico-nano-edge
        echo 'Development machine setup is finished.'
        echo 'Now you can execute: vagrant ssh'"

end
