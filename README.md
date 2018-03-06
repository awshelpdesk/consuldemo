# Consul Redis demo

1. Download the repo as follows, git clone https://github.com/awshelpdesk/consuldemo.git
2. cd into folder 'composetest'
3. Run 'docker-compose up'
4. Register redis service with Consul as follows, make sure you are in folder 'composetest', execute the command 'curl -X PUT --data-binary @./json/redis.json http://localhost:8500/v1/agent/service/register'

Note: We can automate the process lately like mentioned here, https://github.com/subokita/consul-docker-test

5. To verify service from consul registry execute command 'curl http://localhost:8500/v1/catalog/service/redis | jq'
6. Use 'http://<DOCKER_HOST>:8500/ui/#/dc1/services' to browse Consul UI in browser.
