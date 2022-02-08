# -*- mode: ruby -*-
# vi: set ft=ruby :

$script1 = <<-'SCRIPT-1'
# install latest ver terraform https://learn.hashicorp.com/tutorials/terraform/install-cli
which terraform &> /dev/null || {
    echo "installing terraform"
    export DEBIAN_FRONTEND=noninteractive
    apt-get update > /dev/null 2>&1
    apt-get install -y gnupg software-properties-common curl            > /dev/null 2>&1
    curl -fsSL https://apt.releases.hashicorp.com/gpg | apt-key add -   > /dev/null 2>&1
    apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main" > /dev/null 2>&1
    apt-get update                  > /dev/null 2>&1
    apt-get install -y terraform    > /dev/null 2>&1
}
which ruby &> /dev/null || {
    echo "installing ruby"
    export DEBIAN_FRONTEND=noninteractive
    apt-get install -y ruby-full        > /dev/null 2>&1  # https://www.ruby-lang.org/en/documentation/installation/#apt
    apt-get install -y build-essential  > /dev/null 2>&1  # https://stackoverflow.com/questions/54413605/error-occured-while-installing-unf-ext-0-0-7-4-and-bundler-cannot-continue
    gem install bundler                 > /dev/null 2>&1
}

# prepare env for simple test https://newcontext-oss.github.io/kitchen-terraform/getting_started.html
echo "creating directory my_terraform_module under /vagrant"
cd /vagrant
mkdir -p my_terraform_module && cd my_terraform_module
mkdir -p test/integration/kt_suite/controls 
mkdir -p test/fixtures/tf_module/

# install kitchen-terraform https://www.rubydoc.info/gems/kitchen-terraform
echo "installing kitchen-terraform in /vagrant/my_terraform_module"
rm Gemfile &> /dev/null || touch Gemfile
echo 'source "https://rubygems.org/" do' >> Gemfile 
echo '    gem "kitchen-terraform", " > 6.0"' >> Gemfile
echo 'end' >> Gemfile
bundle install > /dev/null 2>&1

echo "creating /vagrant/my_terraform_module/.kitchen.yml"
rm .kitchen.yml &> /dev/null
cat <<-'EOF1' > .kitchen.yml
---
driver:
  name: terraform
  root_module_directory: test/fixtures/tf_module
  parallelism: 4

provisioner:
  name: terraform

verifier:
  name: terraform
  systems:
    - name: basic
      backend: local
      controls:
        - file_check

platforms:
  - name: terraform

suites:
  - name: kt_suite
EOF1

echo "creating /vagrant/my_terraform_module/main.tf"
rm main.tf &> /dev/null
cat <<-'EOF2' > main.tf
resource "null_resource" "create_file" {
  provisioner "local-exec" {
    command = "echo 'this is my first test' > foobar"
  }
}
EOF2

echo "creating /vagrant/my_terraform_module/test/fixtures/tf_module/main.tf"
rm test/fixtures/tf_module/main.tf &> /dev/null
cat <<-'EOF3' > test/fixtures/tf_module/main.tf
module "kt_test" {
    source = "../../.."
}
EOF3

echo "preparing kitchen useful commands for the user"
echo "echo ' '" > /etc/profile.d/kitchen-instruction.sh
echo "echo 'https://newcontext-oss.github.io/kitchen-terraform/getting_started.html'" >> /etc/profile.d/kitchen-instruction.sh
echo "echo ' '" >> /etc/profile.d/kitchen-instruction.sh
echo "echo 'bundle exec kitchen converge'" >> /etc/profile.d/kitchen-instruction.sh
echo "echo 'bundle exec kitchen verify'" >> /etc/profile.d/kitchen-instruction.sh
echo "echo 'bundle exec kitchen destroy'" >> /etc/profile.d/kitchen-instruction.sh
echo "echo ' '" >> /etc/profile.d/kitchen-instruction.sh

SCRIPT-1


Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.hostname = "kitchen"
  config.ssh.extra_args = ["-t", "cd /vagrant/my_terraform_module; bash --login"]
  config.vm.provision "shell", inline: $script1
end