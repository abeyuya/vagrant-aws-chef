# -*- mode: ruby -*-
# vi: set ft=ruby :

# ENV value load from ./.env
# Dotenv.load

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # 起動時設定
  config.ssh.pty = true
  config.omnibus.chef_version = :latest
  config.vm.box     = "dummy"
  config.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # awsインスタンス生成
  config.vm.provider :aws do |aws, override|
    aws.access_key_id        = ENV['AWS_ACCESS_KEY_ID']
    aws.secret_access_key    = ENV['AWS_SECRET_ACCESS_KEY']
    aws.keypair_name         = ENV['AWS_KEYPAIR_NAME']
    aws.region               = ENV['AWS_REGION']
    aws.ami                  = ENV['AWS_AMI']
    aws.security_groups      = ENV['AWS_SECURITY_GROUP']
    aws.instance_type        = "t2.micro"

    override.ssh.private_key_path = ENV['AWS_SSH_PRIVATE_KEY_PATH']
    override.ssh.username         = ENV['AWS_SSH_USERNAME']
  end

  # プロビジョニング
  config.berkshelf.enabled = false
  config.vm.provision :chef_solo do |chef|
    # chef.log_level = :debug
    chef.cookbooks_path = [
      "./chef-aws/cookbooks",
      "./chef-aws/site-cookbooks"
    ]

    chef.json = {
      rbenv: {
        users: [ENV['AWS_SSH_USERNAME']],
        ruby: {
          versions: ["2.1.0"],
          global: "2.1.0"
        },
      },
    }

    chef.run_list = [
      "git",
      "recipe[setup-ec2-middleware]",
      "passenger_apache2",
    ]

  end
end
