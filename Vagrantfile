##### START Helper functions
def install_ssh_key()
    puts "Adding ssh key to the ssh agent"
    puts "ssh-add #{Vagrant.source_root}/keys/vagrant"
    system "ssh-add #{Vagrant.source_root}/keys/vagrant"
  end
  

# Uncomment the next line if you're using ssh-agent
# install_ssh_key

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

    config.ssh.insert_key = false

    (1..2).each do |id|
        re_name  = ( "vqfx" + id.to_s ).to_sym
        srv_name  = ( "srv" + id.to_s ).to_sym

        ##########################
        ## Routing Engine  #######
        ##########################
        config.vm.define re_name do |vqfx|
            vqfx.vm.hostname = "vqfx#{id}"
            vqfx.vm.box = 'juniper/vqfx10k-re'
            # DO NOT REMOVE / NO VMtools installed
            vqfx.vm.synced_folder '.', '/vagrant', disabled: true

            # Management port (em1 / em2)
            vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "vqfx_internal_#{id}"
            vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "reserved-bridge"

            # Dataplane ports (em3)
            vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "server_vqfx#{id}"

            # (em4, em5)
            (1..2).each do |seg_id|
                vqfx.vm.network 'private_network', auto_config: false, nic_type: '82540EM', virtualbox__intnet: "seg#{seg_id}"
            end
        end

        ##########################
        ## Server          #######
        ##########################
        config.vm.define srv_name do |srv|
            srv.vm.box = "minimal/xenial64"
            srv.vm.hostname = "srv#{id}"
            srv.vm.network 'private_network', ip: "10.10.#{id}.2", virtualbox__intnet: "server_vqfx#{id}"
            srv.ssh.insert_key = true
            srv.vm.provision "shell",
               inline: "sudo route add -net 10.10.0.0 netmask 255.255.0.0 gw 10.10.#{id}.1"
        end
    end

    ##############################
    ## Box provisioning    #######
    ##############################
    config.vm.provision "ansible" do |ansible|
        ansible.groups = {
            "vqfx10k" => ["vqfx1", "vqfx2" ],
            "server"  => ["srv1", "srv2" ],
            "all:children" => ["vqfx10k", "server"]
        }
        ansible.playbook = "provisioning/deploy-config.p.yaml"
    end
end
