################################################################################
#
# Vagrantfile
#
################################################################################

# Buildroot version to use
RELEASE='v1.0'

### Change here for more memory/cores ###
VM_MEMORY=4096
VM_CORES=4
VM_NAME="Netico Linux Distro #{RELEASE}"

Vagrant.configure('2') do |config|
	config.vm.box = 'ubuntu/bionic64'

	config.vm.provider :vmware_fusion do |v, override|
		v.vmx['memsize'] = VM_MEMORY
		v.vmx['numvcpus'] = VM_CORES
	end

	config.vm.provider :virtualbox do |v, override|
		v.memory = VM_MEMORY
		v.cpus = VM_CORES

		required_plugins = %w( vagrant-vbguest )
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
			git tig vim bzr cvs mercurial subversion libc6:i386 unzip bc \
			binfmt-support qemu qemu-user-static debootstrap kpartx \
            lvm2 dosfstools gpart binutils bison git lib32ncurses5-dev \
            libssl-dev python-m2crypto gawk wget git-core diffstat unzip \
            texinfo gcc-multilib build-essential chrpath socat libsdl1.2-dev \
            autoconf libtool libglib2.0-dev libarchive-dev python-git xterm \
            sed cvs subversion kmod coreutils texi2html bc docbook-utils \
            python-pysqlite2 help2man make gcc g++ desktop-file-utils \
            libgl1-mesa-dev libglu1-mesa-dev mercurial automake groff curl \
            lzop asciidoc u-boot-tools mtd-utils device-tree-compiler flex \
            debootstrap qemu-arm-static
		apt-get -q -y autoremove
		apt-get -q -y clean
		update-locale LC_ALL=C"

	config.vm.provision 'shell', privileged: false, inline:
		"echo 'Downloading and extracting Linux #{RELEASE}'
		git clone https://github.com/varigit/debian-var.git -b debian_stretch_mx6_var01 var_som_mx6_debian"

end
