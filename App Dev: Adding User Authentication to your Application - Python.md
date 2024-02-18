# ``` App Dev: Adding User Authentication to your Application - Python ```
## Task 1
```bash
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
ln -s ~/training-data-analyst/courses/developingapps/v1.3/python/firebase ~/firebase
cd ~/firebase/start
export REGION=<YOUR-REGION>
sed -i "s/us-central/$REGION/g" prepare_environment.sh
. prepare_environment.sh
```
## Task 3
###### In the Google Cloud Console, on the Navigation menu (Navigation menu icon), click Identity Platform. Click Enable Identity Platform.
###### Click Add a Provider,For Select a provider, select Email / Password, Click Enabled.
###### In the Authorized Domains pane, click Add Domain.You will get command like this in lab console copy and add it in domain
``` 8080-27542cac-44d0-41a9-9e96-065800c2100c.ql-us-east1-ctgq.cloudshell.dev ```
###### Now, in Identity Platform select Users, Add Users given in lab console
