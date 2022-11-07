# services-deployment

## Install services
* Copy this repository.
* Change IPs in the inventory files `hosts`.
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
$ http -f post "your_ip:3000/api/v1/user" "name=bob" "email=bob@example.com" "password=password" # create the new user bob

$ http -f post "your_ip:3000/api/v1/user_session" "email=bob@example.com" "password=password" # get the bob's token

$ http -f post "your_ip:3001/api/v1/ads" "ad[title]=message" "ad[city]=Moscow" "ad[description]=suggestion" "Authorization:Bearer bobs_token" # add a new advert message

$ http -f get ":3001/api/v1/ads" # get message list
```
or
```bash
$ http -f post "your_ip/api/v1/user" "name=john" "email=john@example.com" "password=password"  "Host: auth.example.com" # create the new user john

$ http -f post "your_ip/api/v1/user_session" "email=john@example.com" "password=password"  "Host: auth.example.com" # get the john's token

$ http -f post "your_ip/api/v1/ads" "ad[title]=message" "ad[city]=Moscow" "ad[description]=suggestion" "Authorization:Bearer johns_token"  "Host: ads.example.com" # add a new advert message

http "http://your_ip/api/v1/ads" "Host: ads.example.com" # get message list
```

## Author
* it.Architect https://github.com/rubygitflow
* Inspired by [Evgeniy Fateev](https://github.com/psylone/playbooks-rms)
