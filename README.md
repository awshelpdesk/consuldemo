# Consul Redis demo

1. Download the repo as follows, git clone https://github.com/awshelpdesk/consuldemo.git
2. cd into folder 'composetest'
3. Run 'docker-compose up'
4. Register redis service with Consul as follows, make sure you are in folder 'composetest', execute the command 'curl -X PUT --data-binary @./json/redis.json http://localhost:8500/v1/agent/service/register'
5. To verify service from consul registry execute command 'curl http://localhost:8500/v1/catalog/service/redis | jq'
6. Use 'http://<DOCKER_HOST>:8500/ui/#/dc1/services' to browse Consul UI in browser.

Note: We can automate the discovery process lately, like mentioned here, https://github.com/subokita/consul-docker-test

# Deployment via AWS ECS

1. We need to setup AWS CLI(https://docs.aws.amazon.com/cli/latest/userguide/installing.html) environment with ECS CLI(https://docs.aws.amazon.com/AmazonECS/latest/developerguide/cmd-ecs-cli-compose-up.html).
1a. Configure AWS CLI with access-keys and region information (https://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html)
   
2. To run our docker-compose.yml over ECS, we need to transpose it to correct ecs format using 'convert-docker-compose-to-cloudformation.rb'
2a. As a result you will get a file named 'cloud-formation-task.json' which is an ECS version of local docker-compose.yml

3. Now its time to create ECR repositories and upload our local docker containers over AWS.
3a. Firstly locally run 'docker-compose --build. and note down the generated containers names.
3b. Updated '01-create-ecr-repositories' file, with the correct container names, and execute the script, it will create the ECS repositories over AWS. Don't forget to update AWS_PROFILE, AWS_REGION and ECR_PREFIX before execution.

3c. To build and tag your local docker containers over ECR, execute '02-build-ecr-images'. Don't forget to update containers name with AWS_PROFILE, AWS_REGION, AWS_ACCOUNT_ID and ECR_PREFIX.
