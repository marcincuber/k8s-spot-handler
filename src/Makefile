REGISTRY ?= "public.ecr.aws/b8a8x1o6"
NAME ?= "k8s-spot-handler"

BASE_IMAGE ?= "alpine:3.12.7"
KUBE_VERSION ?= "1.18.20"
REVISION ?= "1"

build_and_push: build_version push_version

build_version:
	docker build -t $(REGISTRY)/$(NAME):$(KUBE_VERSION)-$(REVISION) --build-arg BASE_IMAGE=$(BASE_IMAGE) --build-arg KUBE_VERSION=$(KUBE_VERSION) .

save:
	docker save $(REGISTRY)/$(NAME):$(KUBE_VERSION)-$(REVISION) -o $(NAME)_$(KUBE_VERSION).tar

load:
	docker load -i $(NAME)_$(KUBE_VERSION)-$(REVISION).tar

push_version:
	docker push $(REGISTRY)/$(NAME):$(KUBE_VERSION)-$(REVISION)

image_exists:
	DOCKER_CLI_EXPERIMENTAL=enabled docker manifest inspect $(REGISTRY)/$(NAME):$(KUBE_VERSION)-$(REVISION)
