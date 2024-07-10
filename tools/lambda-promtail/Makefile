app_name     = lambda-promtail
version      = $(or $(CIRCLE_BUILD_NUM), $(VERSION), latest)
ecr_registry = 166248964328.dkr.ecr.eu-west-1.amazonaws.com
image_url    = $(ecr_registry)/$(app_name):$(version)

.PHONY: build
build:
	docker buildx build --platform "linux/amd64" -t $(image_url) .

.PHONY: dev-start
dev-start:
	docker-compose up --build

.PHONY: ecr-login
ecr-login:
	AWS_SECRET_ACCESS_KEY=$$CIRCLE_CI_ECR_SECRET_KEY \
	AWS_ACCESS_KEY_ID=$$CIRCLE_CI_ECR_ACCESS_KEY \
	AWS_DEFAULT_REGION=eu-west-1 \
	aws ecr get-login-password \
			--region eu-west-1 \
	| docker login \
			--username AWS \
			--password-stdin $(ecr_registry)

.PHONY: ecr-publish
ecr-publish:
	docker push $(image_url)

.PHONY: format
format:
	goimports fmt ./...

.PHONY: vet
vet:
	go vet ./...
