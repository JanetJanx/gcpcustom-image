# Custom Ubuntu18 image with Nginx
Creating a custom ubuntu image on gcp using packer and ansible.

## steps
Before creating the custom image, install packer and ansible together with google cloud sdk

### packer installation

Download packer using curl package.

$ curl -O https://releases.hashicorp.com/packer/0.12.2/packer_0.12.2_linux_amd64.zip

Install unzip incase you don't have it installed.
$ sudo apt install -y unzip

Unzip the downloaded packer folder.

$ sudo unzip -d /usr/local/bin packer_0.12.2_linux_amd64.zip

Verify that the installation is successful by checking whether packer is available.

$ packer

### ansible installation

#### On ubuntu 18.04 lts
First run the update command.

$ sudo apt update

Install the software-properties-common package to easily manage ansible and may be other independent softwares.

$ sudo apt install software-properties-common

Add Ansible PPA.

$ sudo apt-add-repository ppa:ansible/ansible

Refresh the system package index by updating.

$ sudo apt update

Install ansible.

$ sudo apt install ansible

[Configure ansible hosts](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-ubuntu-18-04)

#### On ubuntu 16.04 lts
Add Ansible PPA.

$ sudo apt-add-repository ppa:ansible/ansible

Refresh the system package index by updating.

$ sudo apt update

Install ansible.

$ sudo apt install ansible

[Configure ansible hosts](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-ubuntu-16-04#step-1-%E2%80%94-installing-ansible)

### Install and conigure Google Cloud sdk
Create environment variable for the corect distribution of the cloud sdk.

$ export CLOUD_SDK_REPO="cloud-sdk-$(lsb_release -c -s)"

Add the cloud sdk distribution url as a package source

$ echo "deb http://packages.cloud.google.com/apt $CLOUD_SDK_REPO main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list

Download the google cloud platform public key

curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

Update the system package index

$ sudo apt-get update

Install the Cloud SDK

$ sudo apt-get install google-cloud-sdk

Initialize the SDK

gcloud init

And then follow the prompted steps to complete the default configurations.

More about [gcloud sdk](https://cloud.google.com/sdk/docs/quickstart-debian-ubuntu)

### To use the project
Make sure that the above steps are complete and then git clone the project with HTTPS

$ git clone https://github.com/Janet-Namutebi/firstcloud-instance.git

Create the service account json file using the generated json file when creating a key for the service account of your cloud project. Make sure the used service account has the required privilleges.

For security reasons, its recommended to hide your service account file on git and github using the .gitignore file and also create an enironment variable for your cloud project name using the command.

$ export project_id=cloud-projectname

After that, run the packer build command to build the custom image.

$ packer build packerimage.json

| Note: packerimage.json is the name of my project packer file.

### You can also set up your project using the following file structure
            |-- packerfile.json

            |-- service_account_details.json

            |-- roles

            |---- roleone

            |------ tasks

            |--------- main.yml

            |------ templates

            |--------- nginx.conf

            |---- playbookfile.yml