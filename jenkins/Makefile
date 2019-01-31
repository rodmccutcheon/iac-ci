remote_user = ubuntu
ssh_key_name = keys/terransible_provisioner
pipeline_project_name = https://github.com/rodmccutcheon/pipeline-test.git
project_name = 'pipeline-test'
jenkins_auth_user = 'admin'
jenkins_auth_password = 'admin'

ansible:
	ansible-galaxy install -r ansible/requirements.yml -p ansible/roles

keys:
	mkdir keys
	ssh-keygen -t rsa \
		-b 4096 \
		-C terransible_provisioner \
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
	ansible-playbook -i ansible/ec2.py \
		-e 'ansible_python_interpreter=/usr/bin/python3' \
		--private-key=$(ssh_key_name) ansible/main.yml \
		--extra-vars='{"pipeline_repo":"$(repo)", "pipeline_project_name":"$(project_name)", "jenkins_auth_user": "$(jenkins_auth_user)", "jenkins_auth_password": "$(jenkins_auth_password)"}'

jenkins: build provision