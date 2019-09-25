.PHONY: install-boxes install-box-amd64-virtualbox

all: install-boxes

install-boxes: install-box-amd64-virtualbox

install-box-amd64-virtualbox: windows-amd64-virtualbox.box
	vagrant box add -f --name snake-charmers/windows-amd64 --provider virtualbox windows-amd64-virtualbox.box

windows-amd64-virtualbox.box: windows-amd64.json amd64/Autounattend.xml *.ps1 Vagrantfile
	PACKER_LOG=1 packer build -force -only virtualbox-iso windows-amd64.json

clean: clean-packer clean-boxes clean-vagrant clean-artifacts

clean-packer:
	-rm crash.log

clean-boxes:
	-rm *.box

clean-vagrant:
	-rm -rf .vagrant

clean-artifacts:
	-rm -rf packer_cache

lint: packer-validate

packer-validate:
	find . -name '*.json' -exec packer validate {} \;
