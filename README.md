# services-deployment

## Install services
* Copy this repository.
* Change IPs in `hosts` files.
* Add `playbooks/secrets.yml`:
```yml
ads_microservice_password: 
auth_microservice_password: 
ads_microservice_user: 
auth_microservice_user: 
ads_microservice_database: 
auth_microservice_database: 
rabbitmq_password: 
```
* Fix `env_vars` in `playbooks/setup.yml`.
* Run ansible playbooks:
```bash
$ ansible-playbook playbooks/postgresql.yml
$ ansible-playbook playbooks/rabbitmq.yml
$ ansible-playbook playbooks/setup.yml

$ ansible-playbook ads_microservice/deploy/deploy.yml
$ ansible-playbook auth_microservice/deploy/deploy.yml
$ ansible-playbook geocoder_microservice/deploy/deploy.yml
```

## Use services with requests
```bash
$ http -f post "your_ip:3000/api/v1/user" "name=bob" "email=bob@example.com" "password=password" # create a new user

$ http -f post "your_ip:3000/api/v1/user_session" "email=bob@example.com" "password=password" # get user token

$ http -f post "your_ip:3001/api/v1/ads" "ad[title]=message" "ad[city]=Moscow" "ad[description]=suggestion" "Authorization:Bearer some_user_token" # add new advert message
```

## Author
* it.Architect https://github.com/rubygitflow
* Inspired by [Evgeniy Fateev](https://github.com/psylone/playbooks-rms)
