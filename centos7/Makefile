# This should NOT be needed but `type` fails otherwise
SHELL = /bin/bash

# Needed executables
PACKER_EXEC = /opt/packer/packer
MISC_EXEC = jq sed

# Packer config file is the parent directory plus .json suffix
PACKER_CONFIG = "$(shell basename $(PWD)).json"

# Default location for Kickstart config
KICKSTART_CONFIG = "http/ks.cfg"

# Fetches password from Packer config and passes to Kickstart as the root password
ROOT_PASSWORD = $(shell jq '.variables.ssh_password' $(PACKER_CONFIG) | sed -e 's/"//g')

# Verifies if all commands needed are in the PATH
REQUIREMENTS = $(MISC_EXEC) $(PACKER_EXEC)
K := $(foreach exec, $(REQUIREMENTS), $(if $(shell type $(exec) 2>/dev/null)," is ",  \
	$(error Command `$(exec)` not found in PATH)))


all: prepare validate kvm

clean:
	rm -f artifacts/*.box
	rm -f artifacts/*.qcow2
	rm -f artifacts/*.xz
	rm -rf output-virtualbox-iso
	rm -rf packer_cache

prepare:
	sed -i -e 's/^rootpw.*/rootpw $(ROOT_PASSWORD)/g' $(KICKSTART_CONFIG)

validate:
	$(PACKER_EXEC) validate $(PACKER_CONFIG)

kvm:
	$(PACKER_EXEC) validate $(PACKER_CONFIG)
	$(PACKER_EXEC) build $(PACKER_CONFIG)
