# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

# Two box environment
boxes = [
	{
		:name => 'webserver',
		:ram => 512,
		:ansible_playbook => "ansible/webserver.yml",
		:ip => "192.168.200.1",
		:port_forwarded => 8080
	},
	{
		:name => 'elkserver',
		:ram => 2000,
		:ansible_playbook => "ansible/elkserver.yml",
		:ip => "192.168.200.2",
		:port_forwarded => 8081
	}
]



Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

	config.vm.box = "chef/centos-6.5"

	boxes.each do |opts|
		config.vm.define opts[:name] do |machine|
			machine.vm.hostname = opts[:name]
			machine.vm.network "private_network", ip: "#{opts[:ip]}"
			machine.vm.network "forwarded_port", host: opts[:port_forwarded], guest: 80


			machine.vm.provider :virtualbox do |virtual|
				virtual.customize ['modifyvm', :id, '--memory', opts[:ram] ]
			end

			machine.vm.provision "ansible" do |ansible|
				ansible.playbook = "#{opts[:ansible_playbook]}"
				ansible.verbose = 'vvv'
				ansible.sudo = true
			end
		end
	end
end
