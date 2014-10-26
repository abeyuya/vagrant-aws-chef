# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
Dotenv.load
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # 起動時設定
  # vagrant up --provider=aws
  config.vm.box     = "dummy"
  config.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.omnibus.chef_version = :latest

  # awsインスタンス生成
  config.vm.provider :aws do |aws, override|
    aws.access_key_id        = ENV['AWS_ACCESS_KEY_ID']
    aws.secret_access_key    = ENV['AWS_SECRET_ACCESS_KEY']
    aws.keypair_name         = ENV['AWS_KEYPAIR_NAME']
    aws.region               = ENV['AWS_REGION']
    aws.ami                  = ENV['AWS_AMI']
    aws.security_groups      = ENV['AWS_SECURITY_GROUP']
    aws.instance_type        = "t2.micro"
    aws.user_data = <<-USER_DATA
      #!/bin/sh
      sed -i -e 's/^\\(Defaults.*requiretty\\)/#\\1/' /etc/sudoers
    USER_DATA

    override.ssh.private_key_path = ENV['AWS_SSH_PRIVATE_KEY_PATH']
    override.ssh.username         = ENV['AWS_SSH_USERNAME']
  end

  # プロビジョニング
  config.berkshelf.enabled = false
  config.vm.provision :chef_solo do |chef|
    chef.log_level = :debug
    chef.cookbooks_path = [
      "./chef-aws/cookbooks",
      "./chef-aws/site-cookbooks"
    ]

    chef.json = {
      rbenv: {
        rubies: ["2.1.1"],
        global: "2.1.1"
      },
      mysql: {
        server_root_password: ''
      },
    }

    chef.run_list = [
      "ruby_build",
      # "rbenv::vagrant",
      # "rbenv::system",
      # "passenger_apache2",
    ]

  end

end
