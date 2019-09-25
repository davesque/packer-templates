.PHONY: install-boxes install-box-amd64-virtualbox install-boxes

all: install-boxes

install-boxes: install-box-amd64-virtualbox

install-box-amd64-virtualbox: debian-amd64-virtualbox.box
	vagrant box add -f --name snake-charmers/debian-amd64 --provider virtualbox debian-amd64-virtualbox.box

debian-amd64-virtualbox.box: debian-amd64.json http/p *.sh
	PACKER_LOG=1 packer build -force -only virtualbox-iso debian-amd64.json

clean: clean-boxes clean-vagrant clean-artifacts

clean-boxes:
	-rm *.box

clean-vagrant:
	-rm -rf .vagrant

clean-artifacts:
	-rm -rf packer_cache

lint: packer-validate shfmt

packer-validate:
	find . -name '*.json' -exec packer validate {} \;

shfmt:
	find . -name '*.sh' -print | xargs shfmt -w -i 4
