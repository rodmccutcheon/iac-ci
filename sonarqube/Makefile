remote_user = ubuntu
ssh_key_name = keys/id_rsa_sonarqube

ansible:
	ansible-galaxy install -r ansible/requirements.yml -p ansible/roles

keys:
	mkdir keys
	ssh-keygen -t rsa \
		-b 4096 \
		-C terransible_provisioner_sonarqube \
		-f $(ssh_key_name) \
		-P ''

clean:
	rm -rf ansible/roles/
	rm -rf keys

build: keys
	terraform apply -auto-approve

provision: ansible
	ANSIBLE_HOST_KEY_CHECKING=false \
	ANSIBLE_REMOTE_USER=$(remote_user) \
	ANSIBLE_PRIVATE_KEY_FILE=$(ssh_key_name) \
	ansible-playbook -i ansible/aws_ec2.yml ansible/main.yml -e "ansible_python_interpreter=/usr/bin/python2.7"

sonarqube: build provision