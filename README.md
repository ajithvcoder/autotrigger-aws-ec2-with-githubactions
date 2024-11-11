### Auto-Trigger-AWS-EC2-with-GPU

### Request AWS Spot instance (optional)
- Since spot instance is budget friendly i am explaining this blog with spot instance example

- Go to Searvice Quota -> AWS Services -> Amazon Elastic Compute Client(Amazon Ec2)
- Change the region to Mumbai ap-south-1 (nearest to your location)

    ![ec2_quota](./assets/snap_aws_ec2_service_quota.png)

- I need g4dn.xlarge so i am going to request "All G and VT Spot Instance Requests" in Quota name.

    ![g_and_t_instance](./assets/snap_g_and_t_instance.png)

- After clicking "All G and VT Spot Instance Requests" name you need to click "Request increase at account level" and you can give 8 or 16 or 32 as input.
    ![request_limit](./assets/snap_request_limit.png)

    Note: Spot instance takes 1 week for approval but the GPU cost would be 1/10 of the original gpu cost. Dont leave reading this blog because of that for running this experiment without spot instance takes less than 1 USD only.

### Credentials Configuration

Note: We can create roles or policies and proceed for specific permission I didnt explore it.

- Go to "I AM" dashboard and click the user that has account level permission

    ![request_limit](./assets/snap_iam_dashboard.png)

- Make sure it has "AdministratorAccess" perimission

    ![request_limit](./assets/snap_permission.png)

- Click "Create Access Key" -> "Application running outside AWS" -> "Create access key" -> Download .csv file

    ![request_limit](./assets/snap_access_permission.png)

- if you look at .csv file `Access key ID` and `Secret access key`

**Github Repo**

- Go to github repo where the workflow is going to run. Go to Settings -> "Secret and variables" -> Actions -> New repository secrets

- Give AWS_ACCESS_KEY_ID and its value like below and cliek `Add secret` from your .csv file

    ![request_limit](./assets/snap_secret_store.png)

- Similarly store `AWS_SECRET_ACCESS_KEY` in `repository secrets`


- Store the fine_graded_personal-access_token generated got from your github account in the name `PERSONAL_ACCESS_TOKEN`. To run workflow you need additional privilage so better enable every checkbox when creating fine_graded_pak

    ![request_limit](./assets/snap_after_entering.png)

**Github Workflow**

There are many tools to start and stop AWS instances and services.

We are going to use [cml-runner](https://github.com/iterative/setup-cml?tab=readme-ov-file#setup-cml-action) to trigger EC2 spot instance. Below is the trigger command.

```
cml runner launch --cloud=aws --name=session-08 \
    --cloud-region=ap-south-1 --cloud-type=g4dn.xlarge --cloud-hdd-size=64 \
    --cloud-spot --single --labels=cml-gpu --idle-timeout=100
```
`--idle-timeout=100` means after 100 seconds of idle time the instance shuts down

#todo instance starts

- With some verification methods `http://169.254.169.254/latest/meta-data/instance-type` we can confirm if we had triggered the similar instance and if GPU is also attached to it. todo link

- Since aws cli is not installed by default as we are not using specific CLI we can instance aws cli manually and configure.


#todo cli runs

- After execution and `timeout-minutes: 10 minutes` the instance shutsdown and terminates automatically.

#todo instance shuts down
