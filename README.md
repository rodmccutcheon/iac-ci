terraform apply -auto-approve
ansible-galaxy install -r requirements.yml
ansible-playbook main.yml -i inventory -K