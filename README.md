# Deploy an inference API on AWS (EC2) using FastAPI Docker and Github Actions

## Launch an EC2 instance

## Configure a Gihub Actions workflow

1- Go to your repo and click on the **Actions** tab

<p align="center">
    <img src="./images/ga_1.png"/>
</p>

2. Click on **setup a workflow yourself**

<p align="center">
    <img src="./images/ga_2.png"/>
</p>

3. Define your workflow

A YAML file will be automatically created inside a `workflows` folder which will be itself created in a `.github` folder at the root of the repo.

The workflow will be triggered on **push requests** only (on the main branch)

<p align="center">
    <img src="./images/ga_3.png"/>
</p>

The job that will be triggered will be run on a remote server that Github Actions will connect to through the **SSH Remote Commands** custom Github Action that you can find from the marketplace.

<p align="center">
    <img src="./images/ga_4.png"/>
</p>

The **SSH Remote Commands** Github Action will be called with the following arguments

- host: the hostname of the server (i.e. its public IP)
- username: the ssh username
- key: the content of ssh private key
- script: the script that will be executed once the ssh connection is established

The script will list the commands that will be run on the server once the SSH connection is established: it'll clone the repo, cd into it and run the docker-compose build and up commands.

```bash
git clone git@github.com:ahmedbesbes/anonymization-api.git
cd anonymization-api
sudo docker-compose build
sudo docker-compose up -d
```

4. Define Github secrets

The previous arguments `host`, `username` and `key` will not be hard-coded in the YAML file.

They will rather be stored as Github secrets and referenced with the $ sign, the same way you would call environment variables.

To create Github secrets, go to the settings of the repository and click on **Secrets** on the left tab.

<p align="center">
    <img src="./images/ga_5.png"/>
</p>

Then define your secrets by giving setting their name (in capital letters) and their value.

<p align="center">
    <img src="./images/ga_6.png"/>
</p>

Here's how you would set the `USERNAME` secret for example.

<p align="center">
    <img src="./images/ga_7.png"/>
</p>

5. Commit, push and look out for the magic happening

Once you push your code (after testing that everything works fine locally) you will notice a new run queued to start.

<p align="center">
    <img src="./images/ga_8.png"/>
</p>

By clicking on it, you can see the different steps of the build.

<p align="center">
    <img src="./images/ga_9.png"/>
</p>

6. Check that everything is working

Once the API is successfully deployed on your remote server, fire up Postman and execute some requests on the API endpoint.

<p align="center">
    <img src="./images/postman.png"/>
</p>