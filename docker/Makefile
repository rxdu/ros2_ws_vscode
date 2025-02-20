##########################################################################
# Reference: 
# [1] https://developer.nvidia.com/embedded/jetpack-archive
# [2] https://catalog.ngc.nvidia.com/orgs/nvidia/containers/l4t-base/tags
#
##########################################################################

###############################################
# targets
###############################################

.PHONY: all

# base images
all: jammy_humble_run jammy_humble_dev

###############################################
# create builder instance
###############################################

# create buildder instance
builder_instance:
	docker buildx create \
	--name multi-platform \
    --platform linux/amd64,linux/arm64 \
    --driver docker-container

###############################################
# commands
###############################################

_build_multiarch:
	docker buildx build \
		--build-arg BASE_IMAGE=$(base_image) \
		--output "type=image,push=true" \
		--platform linux/amd64,linux/arm64 \
		--builder multi-platform \
		-t ${target_image} \
		. -f ${dockerfile}

_build_arm64:
	docker buildx build \
		--build-arg BASE_IMAGE=$(base_image) \
		--output "type=image,push=true" \
		--platform linux/arm64 \
		--builder multi-platform \
		-t ${target_image} \
		. -f ${dockerfile}

_build_amd64:
	docker buildx build \
		--build-arg BASE_IMAGE=$(base_image) \
		--output "type=image,push=true" \
		--platform linux/amd64 \
		--builder multi-platform \
		-t ${target_image} \
		. -f ${dockerfile}

###############################################
# build and push image
###############################################

.PHONY: jammy_humble_run
jammy_humble_run:
	${MAKE} _build_multiarch \
	base_image=ubuntu:jammy-20230804 \
	target_image=rxdu/ubuntu:run \
	dockerfile=./Dockerfile.run

.PHONY: jammy_humble_dev
jammy_humble_dev: jammy_humble_run
	${MAKE} _build_amd64 \
	base_image=rxdu/ubuntu:run \
	target_image=rxdu/ubuntu:dev \
	dockerfile=./Dockerfile.dev
