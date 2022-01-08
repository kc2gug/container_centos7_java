include make_env

next_version=$(shell echo "$(version) 0.1" | awk '{print $$1 + $$2}' )


build: 
	-rm -f openjdk-17.0.1_linux-x64_bin.tar.gz
	-wget https://download.java.net/java/GA/jdk17.0.1/2a2082e5a09d4267845be086888add4f/12/GPL/openjdk-17.0.1_linux-x64_bin.tar.gz
	docker build --no-cache -t $(repo_host)/$(org_name)/$(repo_name):latest -f Dockerfile .

tag:
	docker tag $(repo_host)/$(org_name)/$(repo_name):latest $(repo_host)/$(org_name)/$(repo_name):v$(version)

push: tag
	docker push $(repo_host)/$(org_name)/$(repo_name):latest
	docker push $(repo_host)/$(org_name)/$(repo_name):v$(version)

stop:
	-docker stop $(shell docker ps -a | grep $(LOGNAME)-$(repo_name) | rev | cut -d " " -f1 | rev)

clean: stop
	-docker  rm  $(shell docker ps -a | grep $(LOGNAME)-$(repo_name) | rev | cut -d " " -f1 | rev)

clean-all: clean
	-docker  rmi $(shell docker images | grep $(repo_host)/$(org_name)/$(repo_name) | grep $(repo_name) | awk '{print $$1":"$$2}' )
	
release: re-version 
	make build
	make push

re-version: 
	sed -i s/"version=$(version)"/"version=$(next_version)"/ make_env

run: 
	docker run --name $(RUN_ID)-$(LOGNAME)-$(repo_name)-$(version) $(SHELLARGS) $(repo_host)/$(org_name)/$(repo_name):latest

deploy:
	make run -e RUN_ID=$@

default: build
