### Consul Redis demo

- [x] 1. Download the repo as follows, `git clone https://github.com/awshelpdesk/consuldemo.git`
- [x] 2. cd into folder `composetest`
- [x] 3. Run `docker-compose up`
- [x] 4. Register redis service with Consul as follows, make sure you are in folder `composetest`, execute the command `curl -X PUT --data-binary @./json/redis.json http://localhost:8500/v1/agent/service/register`
- [x] 5. To verify service from consul registry execute command `curl http://localhost:8500/v1/catalog/service/redis | jq`
- [x] 6. Use `http://<DOCKER_HOST>:8500/ui/#/dc1/services` to browse Consul UI in browser.

Note: We can automate the discovery process lately, like mentioned here, https://github.com/subokita/consul-docker-test

### Deployment via AWS ECS

- [x] 1. We need to setup AWS CLI(`https://docs.aws.amazon.com/cli/latest/userguide/installing.html`) environment with ECS CLI(`https://docs.aws.amazon.com/AmazonECS/latest/developerguide/cmd-ecs-cli-compose-up.html`).

     - [x] Configure AWS CLI with access-keys and region information (`https://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html`)

- [x] 2. To run our `docker-compose.yml` over ECS, we need to transpose it to correct ecs format using `convert-docker-compose-to-cloudformation.rb`

     - [x] As a result you will get a file named `cloud-formation-task.json` which is an ECS version of local `docker-compose.yml`

- [x] 3. Now its time to create ECR repositories and upload our local docker containers over AWS.

     - [x] Firstly locally run `docker-compose --build`. and note down the generated containers names.

     - [x] Update `01-create-ecr-repositories` file, with the correct container names, and execute the script, it will create the ECS repositories over AWS. Don't forget to update `AWS_PROFILE`, `AWS_REGION` and `ECR_PREFIX` before execution.

     - [x] To build, tag and push your local docker containers over ECR, execute `02-build-ecr-images`. Don't forget to update `AWS_PROFILE`, `AWS_REGION`, `AWS_ACCOUNT_ID` and `ECR_PREFIX`.
     
- [x] 4 -. Create an IAM role for ECS components to work in a secure manner, execute `03-create-ecs-role`. Don't forget to update `AWS_PROFILE` and `ROLE_NAME`.

- [x] 5 - Its time to create ECS cluster, execute `04-create-cluster`. Update `AWS_PROFILE` and `CLUSTER_NAME` before execution of script.

- [x] 6 - Create ec2 instance for running ECS Agent and register it with the ECS cluster.  Execute `05-create-ec2` but first update values like `AWS_IAM_INSTANCE_PROFILE`, `AWS_KEY_NAME`, `AWS_PROFILE`, `AWS_SECURITY_GROUP_IDS`, `AWS_SUBNET_ID`, `EC2_COUNT`, `EC2_INSTANCE_TYPE` and `ECS_AMI_ID`. Update cluster name in `ecs.config` too.

- [x] 7 - Its time to register task-definition, so execute `06-register-task`. Update the variables in the fle before execution.

- [x] 8. In last, to execute the ECS task as service upon ec2 instance, update values and execute `07-create-service`. 
