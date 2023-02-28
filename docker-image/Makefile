
IMAGE_NAME ?= $(USER)/ansible
IMAGE_TAG ?= latest
UID = $(shell id -u)

build: requirements.txt
	docker build --build-arg USER_UID=${UID} -t ${IMAGE_NAME}:${IMAGE_TAG} .

requirements.txt:
	docker run --rm python:3.11-slim-bullseye bash -c "pip -qqq install --upgrade pip setuptools wheel && pip -qqq install --user ansible ansible-lint docker proxmoxer requests && pip freeze" > requirements.txt
