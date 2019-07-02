BASE_IMAGE ?= "alpine:3.9.4"
REGISTRY ?= "marcincuber"
VERSION ?= "1.15.0"
NAME ?= "k8s-spot-handler"

build_version:
	docker build -t $(REGISTRY)/$(NAME):$(VERSION) --build-arg BASE_IMAGE=$(BASE_IMAGE) --build-arg VERSION=$(VERSION) .

save:
	docker save $(REGISTRY)/$(NAME):$(VERSION) -o $(NAME)_$(VERSION).tar

load:
	docker load -i $(NAME)_$(VERSION).tar

push_version:
	docker push $(REGISTRY)/$(NAME):$(VERSION)

image_exists:
	DOCKER_CLI_EXPERIMENTAL=enabled docker manifest inspect $(REGISTRY)/$(NAME):$(VERSION)
