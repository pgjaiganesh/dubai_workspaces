.PHONY: all image push

bucket = ganeshji-mena
region = eu-west-1
stack-name = audio-streaming1
project-name = audio-streaming
version = v1

updatestack: copytemplate
	aws cloudformation update-stack --stack-name $(stack-name) --region $(region) --capabilities CAPABILITY_NAMED_IAM \
	--template-url https://s3.amazonaws.com/ganeshji-mena/templates/$(project-name)/$(version)/deploy.yaml \
	--parameters ParameterKey=ChannelName,ParameterValue=$(stack-name)

createstack: copytemplate
	aws cloudformation create-stack --stack-name $(stack-name) --template-body file://dist/deploy.yaml \
	--capabilities CAPABILITY_NAMED_IAM --region $(region) \
	--parameters ParameterKey=ChannelName,ParameterValue=$(stack-name)

package:
	mkdir -p dist && cd lambda/imports3-function && zip -FS -q -r ../../dist/imports3-function.zip *
	mkdir -p dist && cd lambda/dynamodb-function && zip -FS -q -r ../../dist/dynamodb-function.zip *

copycode:
	aws s3 cp dist/ s3://$(bucket)/code/$(project-name)/$(version)/ --acl public-read --recursive;

copytemplate:
	cp templates/deploy.yaml dist/deploy.yaml
	aws s3 cp dist/deploy.yaml s3://$(bucket)/templates/$(project-name)/$(version)/ --acl public-read

zip:
	zip -FS -q -r dist/artifacts.zip container/* Dockerfile-list Dockerfile-migrate Dockerfile-list buildspec.yaml
