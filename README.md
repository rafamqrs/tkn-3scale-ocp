# tkn-3scale-ocp
Private Repo to be used for 3scale and Openshift Pipelines, in this repo are the necessaries tasks to run each one.



# Procedure to create a CD with 3scale toolbox

### Need to create the .3scalerc.yaml file or use the auth URL.
```
$ 3scale remote add https://<TOKEN>@3scale-admin....
```

### Create a CM or Secret with the file generated in the previous steps
```
$ oc create cm toolbox-config \
  --from-file=.3scalerc.yaml=$HOME/.3scalerc.yaml
```

### Create/Update the service with the command
Command: 3scale import openapi -d **<remote> <file_or_url_openapi> --override-private-base-url=<api_url> -t <system_name>**
```
$ 3scale import openapi -d 3scale-saas swagger.json --override-private-base-url=https://echo-api.3scale.net -t beer-catalog
```

### Create/Update an application plan
Command: 3scale application-plan apply **<remote> <system_name> <plan> -n <plan_name> --default(make default app plan).**
```
$ 3scale application-plan apply 3scale-saas beer-catalog test -n "Test Plan" --default
```
### Create/Update an application
Command: 3scale application apply **<remote> <user_key> --account=<account_user> --name=<application_name> --description=<description> --plan=<plan> --service=<system_name> **
```
$ 3scale application apply 3scale-saas 1234567890abcdef --account=john --name="Test Application" --description="Created from the CLI" --plan=test --service=beer-catalog
```
### Promote the configuration to the production gateway:
Command: 3scale proxy-config promote **<remote> <system_name>**
```
3scale proxy-config promote 3scale-saas beer-catalog
```
