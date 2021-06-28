# REF: https://github.com/LINKIT-Group/dockerbuild/blob/master/Makefile
# Make searches for this file first per make default search hierarchy

SHELL := /bin/bash

PWD 									?= pwd_unknown

THIS_FILE								:= $(lastword $(MAKEFILE_LIST))
export THIS_FILE
TIME									:= $(shell date +%s)
export TIME

ifeq ($(user),)
## USER retrieved from env, UID from shell.
HOST_USER								?=  $(strip $(if $(USER),$(USER),nodummy))
HOST_UID								?=  $(strip $(if $(shell id -u),$(shell id -u),4000))
#BY PASS host user 
#HOST_USER								= root
#HOST_UID								= $(strip $(if $(uid),$(uid),0))
else
# allow override by adding user= and/ or uid=  (lowercase!).
# uid= defaults to 0 if user= set (i.e. root).
HOST_USER								= $(user)
HOST_UID								= $(strip $(if $(uid),$(uid),0))
endif
export HOST_USER
export HOST_UID

# PROJECT_NAME defaults to name of the current directory.
ifeq ($(project),)
PROJECT_NAME							:= $(notdir $(PWD))
else
PROJECT_NAME							:= $(project)
endif
export PROJECT_NAME

#GIT CONFIG
GIT_USER_NAME							:= $(shell git config user.name)
export GIT_USER_NAME
GIT_USER_EMAIL							:= $(shell git config user.email)
export GIT_USER_EMAIL
GIT_SERVER								:= https://github.com
export GIT_SERVER
GIT_PROFILE								:= randymcmillan
export GIT_PROFILE
GIT_BRANCH								:= $(shell git rev-parse --abbrev-ref HEAD)
export GIT_BRANCH
GIT_HASH								:= $(shell git rev-parse --short HEAD)
export GIT_HASH
GIT_PREVIOUS_HASH						:= $(shell git rev-parse --short master@{1})
export GIT_PREVIOUS_HASH
GIT_REPO_ORIGIN							:= $(shell git remote get-url origin)
export GIT_REPO_ORIGIN
GIT_REPO_NAME							:= $(PROJECT_NAME)
export GIT_REPO_NAME
GIT_REPO_PATH							:= $(HOME)/$(GIT_REPO_NAME)
export GIT_REPO_PATH

BITCOIN_CONF							:= $(HOME)/$(PROJECT_NAME)/conf/bitcoin.conf
export BITCOIN_CONF

ifneq ($(bitcoin-datadir),)
BITCOIN_DATA_DIR						:= $(bitcoin-datadir)
else
BITCOIN_DATA_DIR						:= $(HOME)/.bitcoin
endif
export BITCOIN_DATA_DIR

ifeq ($(nocache),true)
NOCACHE								:= --no-cache
else
NOCACHE								:=	
endif
export NOCACHE

ifeq ($(verbose),true)
VERBOSE									:= --verbose
else
VERBOSE									:=	
endif
export VERBOSE

ifeq ($(nodeport),)
NODE_PORT								:= 8333
else
NODE_PORT								:= $(nodeport)
endif
export NODE_PORT

ifneq ($(passwd),)
PASSWORD								:= $(passwd)
else 
PASSWORD								:= changeme
endif
export PASSWORD

ifeq ($(cmd),)
CMD_ARGUMENTS							:= 	
else
CMD_ARGUMENTS							:= $(cmd)
endif
export CMD_ARGUMENTS

.PHONY: help
help: report
	@echo ''
	@echo ''
	@echo ''

.PHONY: report
report:
	@echo ''
	@echo '	[ARGUMENTS]	'
	@echo '      args:'
	@echo '        - HOME=${HOME}'
	@echo '        - PWD=${PWD}'
	@echo '        - THIS_FILE=${THIS_FILE}'
	@echo '        - TIME=${TIME}'
	@echo '        - HOST_USER=${HOST_USER}'
	@echo '        - HOST_UID=${HOST_UID}'
	@echo '        - SERVICE_TARGET=${SERVICE_TARGET}'
	@echo '        - PROJECT_NAME=${PROJECT_NAME}'
	@echo '        - DOCKER_BUILD_TYPE=${DOCKER_BUILD_TYPE}'
	@echo '        - GIT_USER_NAME=${GIT_USER_NAME}'
	@echo '        - GIT_USER_EMAIL=${GIT_USER_EMAIL}'
	@echo '        - GIT_SERVER=${GIT_SERVER}'
	@echo '        - GIT_PROFILE=${GIT_PROFILE}'
	@echo '        - GIT_BRANCH=${GIT_BRANCH}'
	@echo '        - GIT_HASH=${GIT_HASH}'
	@echo '        - GIT_PREVIOUS_HASH=${GIT_PREVIOUS_HASH}'
	@echo '        - GIT_REPO_ORIGIN=${GIT_REPO_ORIGIN}'
	@echo '        - GIT_REPO_NAME=${GIT_REPO_NAME}'
	@echo '        - GIT_REPO_PATH=${GIT_REPO_PATH}'
	@echo '        - BITCOIN_CONF=${BITCOIN_CONF}'
	@echo '        - BITCOIN_DATA_DIR=${BITCOIN_DATA_DIR}'
	@echo '        - NOCACHE=${NOCACHE}'
	@echo '        - VERBOSE=${VERBOSE}'
	@echo '        - NODE_PORT=${NODE_PORT}'
	@echo '        - PASSWORD=${PASSWORD}'
	@echo '        - CMD_ARGUMENTS=${CMD_ARGUMENTS}'

#######################

ORIGIN_DIR:=$(PWD)
export ORIGIN_DIR

.PHONY: super
super:
ifneq ($(shell id -u),0)
	@echo switch to superuser
	@echo cd $(TARGET_DIR)
	#sudo ln -s $(PWD) $(TARGET_DIR)
#.ONESHELL:
	sudo -s
endif
#######################
#######################
# Backup $HOME/.bitcoin
########################
#backup:
#	@echo ''
#	bash -c 'mkdir -p $(HOME)/.bitcoin'
##	bash -c 'conf/get_size.sh'
#	bash -c 'tar czv --exclude=*.log --exclude=banlist.dat \
#			--exclude=fee_exstimates.dat --exclude=mempool.dat \
#			--exclude=peers.dat --exclude=.cookie --exclude=database \
#			--exclude=.lock --exclude=.walletlock --exclude=.DS_Store\
#			-f $(HOME)/.bitcoin-$(TIME).tar.gz $(HOME)/.bitcoin'
#	bash -c 'openssl md5 $(HOME)/.bitcoin-$(TIME).tar.gz > $(HOME)/bitcoin-$(TIME).tar.gz.md5'
#	bash -c 'openssl md5 -c $(HOME)/bitcoin-$(TIME).tar.gz.md5'
#	@echo ''
#
#######################
.PHONY: init
init: report
ifneq ($(shell id -u),0)
	@echo 'sudo make init #try if permissions issue'
endif
	@echo 'init'
	git config --global core.editor vim
	@echo ''
	bash -c 'mkdir -p $(BITCOIN_DATA_DIR)'
	bash -c 'mkdir -p /usr/local/bin'
	bash -c 'mkdir -p /usr/local/include'
ifneq ($(shell id -u),0)
	#sudo bash -c 'install -v $(PWD)/conf/usr/local/bin/*  /usr/local/bin'
else
	#bash -c 'install -v $(PWD)/conf/usr/local/bin/*  /usr/local/bin'
endif
	@echo ''
	#bash -c '$(GIT_REPO_PATH)/conf/config.bitcoin.conf.sh'
	@echo ''
	bash -c 'install -v $(PWD)/conf/bitcoin.conf    $(BITCOIN_DATA_DIR)/bitcoin.conf'
ifneq ($(shell id -u),0)
	sudo bash -c 'mkdir -p /usr/local/include/'
endif
	@echo ''
#######################
.PHONY: build
build: report
	@echo 'build'
	$(DOCKER_COMPOSE) $(VERBOSE) build $(NOCACHE) statoshi
	@echo ''
#######################
.PHONY: run
run: build
	@echo 'run'
ifeq ($(CMD_ARGUMENTS),)
	@echo ''
	#
	@echo ''
else
	@echo ''
	#
	@echo ''
endif
#######################
.PHONY: torproxy
torproxy:
	@echo ''
	#REF: https://hub.docker.com/r/dperson/torproxy
	#bash -c 'docker run -it -p 8118:8118 -p 9050:9050 -p 9051:9051 -d dperson/torproxy'
	@echo ''
ifneq ($(shell id -u),0)
	bash -c 'sudo make torproxy user=root &'
endif
ifeq ($(CMD_ARGUMENTS),)
	docker-compose $(VERBOSE) -f docker-compose.yml -p $(PROJECT_NAME)_$(HOST_UID) run --publish 8118:8118 --publish 9050:9050  --publish 9051:9051 --rm torproxy
else
	docker-compose $(VERBOSE) -f docker-compose.yml -p $(PROJECT_NAME)_$(HOST_UID) run --publish 8118:8118 --publish 9050:9050  --publish 9051:9051 --rm torproxy sh -c "$(CMD_ARGUMENTS)"
endif
	@echo ''
#######################
.PHONY: clean
clean:
	# remove created images
	@docker-compose -p $(PROJECT_NAME)_$(HOST_UID) down --remove-orphans --rmi all 2>/dev/null \
	&& echo 'Image(s) for "$(PROJECT_NAME):$(HOST_USER)" removed.' \
	|| echo 'Image(s) for "$(PROJECT_NAME):$(HOST_USER)" already removed.'
	@rm -f torproxy
	@rm -f Makefile
#######################
.PHONY: prune
prune:
	@echo 'prune'
	docker-compose -p $(PROJECT_NAME)_$(HOST_UID) down
	docker system prune -af
#######################
.PHONY: prune-network
prune-network:
	@echo 'prune-network'
	docker-compose -p $(PROJECT_NAME)_$(HOST_UID) down
	docker network prune -f
#######################
-include Makefile

