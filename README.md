# hygieia-jenkins-github-starter

This is a starter project which spins up docker containers of

- Hygieia
    - Github collector
    - Jenkins collector
    - Sonar collector
- Jenkins

## Pre requisites

- `docker` install from [here](https://docs.docker.com/install/)
- `docker-compose` from [here](https://docs.docker.com/compose/install)

## How to Setup

Setup is done in majorly 3 steps

- clone this repository and run containers
- setup Jenkins
- setup Hygieia

#### Clone Repo and run contianers

- Run following command
```sh
git clone https://github.com/subhadippramanik/hygieia-jenkins-github-starter.git
```
- This will create a directory `hygieia-jenkins-github-starter` whithin your current working directory. Navigate to repository using command
```sh
cd hygieia-jenkins-github-starter
```
- Set environment variables for data and log persistence. This needs sharing volume to docker with your host os. Open `.env` file and edit the environment variables by providing the paths of your choice.
- Run containers using command
```sh
docker-compose-up -d
```
This may initially take some time to pull all required images if not present already.

#### Setup Jenkins

Once your containers starts up first setup Jenkins.

- Jenkins will be available at 
```cmd
http://yourhost:5050
```
The first page will ask you setup admin for which you'll need admin password which can be found at
`${JENKINS_HOME}/secrets/initialAdminPassword`
- Next you create admin user and add plugins of your choice. For the demo purpose the admin user name and password needs to be 
```json
username: admin
password: admin
```
If you want to change it then consider changing it in [docker compose](docker-compose.yml#L31) as well before starting it.
- Instance Configuration page will ask you for the Jenkins URL which you need to set as
```cmd
http://jenkins:8080
```
- After completeing admin setup, create the Jenkins job(s)

#### Setup Hygieia

In this step, you need to setup Hygiea.
- Hygieia will be available at 
```cmd
http://yourhost:80/#/signup
```
- First create admin user and sign in
- The next screen is the dashboard where initially you need to create one.
- Go to `Create a new dashboard`
- A form will popup where you set
```sh
Dashboard Type: Team dashboard
Select Widgets 
Dashborad title: ${YOUR DASHBOARD TITLE}
Application name: ${YOUR APP NAME}
```

- and then press `Create` button

- This will open another popup for `Widget Management` which will have lots of widget types for selection
- For now select `build` and `repo` additionally you can also select `sonar` if you have configured it in [docker-compose](docker-compose.yml#L33)
- This will bring you to an empty team dashboard with configurable widgets
- Configure the `Build` widget by selecting build job which automatically shows up in the drop down. You also can set the poll interval and failed build action.
- Configure `Repo` widget by selecting `RepoType` as `GitHub` and then provide your repository url, branch and credentials.

This completes the setup for starter.

#### Configure with existing Jenkins
This can easily be configured with any exisitng Jenkins server with minimum changes
- Change the Jenkins configuration in [docker-compose](docker-compose.yml#L31)
- In addition you can remove the line [3](docker-compose.yml#L3) to [11](docker-compose.yml#L11)