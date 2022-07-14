# ProjectDokerization--CICD
Containerising the hello world app , setting up CICD pipeline and deploying the docker image on HEROKU





Softwares Required:-

1. [Github Account](https://github.com)
2. [Heroku Account](https://dashboard.heroku.com/login)
3. [VS Code IDE](https://code.visualstudio.com/download)
4. [GIT cli](https://git-scm.com/downloads)
5. [GIT Documentation](https://git-scm.com/docs/gittutorial)
6. [DOCKER SOFTWARE](https://docs.docker.com/desktop/install/mac-install/)


ONCE YOU HAVE CREATED A REPO IN GITHUB  AND CLONED IT , FOLLOW THE BELOW STEPS:-

STEPS:-
1) create conda environment

step i)
#conda --version  //to check weather conda environment is present or not

step ii)
#conda create -p venv python==3.7 -y

"""
-p is a prefix which indicates that folder with the name venv will be created in the project directory , and tis virtual env will be deleted as soon as we delete the project . If we give -m this directory will be created where anaconda is present.
"""

step iii)
#conda activate venv/



2) create requirements.txt file

requirements.txt contains list of all libraries or packages that we will need in the project.

pip install -r requirements.txt





3) create Dockerfile
i) which python version we want in our virtual  m/c .
FROM python:3.7

ii) In our virtual m/c there will be a folder called app , so we want to copy files from current directory in that directory
COPY . /app

**** some files and folders like .gitignore , venv , hidden file like .git we dont want in app folder so we will create a .dockerignore file and list down all the files which we dont want in our docker image

iii) now we will make app as our current working directory
WORKDIR /app

iv) now install all the required libraries in new virtual m/c
RUN pip install -r requirements.txt

v) we will set the port on which our application will run
EXPOSE $PORT

vi)Finally ,which file is needed to run the application i.e. app.py
CMD gunicorn --workers=4 --bind <ip-address>.$PORT app:app 

example:-CMD gunicorn --workers=4 --bind 0.0.0.0.$PORT app:app 

****here 0.0.0.0 is local IP




4)Deploying Dockerised project in heroku

i) create .github folder and inside that workflows folder and inside tat create a file called main.yaml file.

3) TO connect CICD pipeline with heroku wen need 5 things :-
i) heroku login email = shubham.datascience29@gmail.com
ii) heroku api key = <dont keep it public>
iii) heroku app = cicd-app123
iv)location of Dockerfile
v)name of Dockerfile

In HEROKU for
****heeroku api key - [ topRightCorner under profile ->  accountSetting -> apikey -> reveal]
****heroku app = [click on new -> create new app -> give some name]


Github will allocate a virtual ubuntu m/c( as mentioned in main.yaml file) where docker image will be build using github action and for deploying that docker image on heroku we need 5 things which I already mentioned above.

Now we will prepare the secrets

Go to ur created repo -> settings -> secrets -> actions -> new repository name 

NAME                    VALUE
HEROKU_EMAIL            shubham.datascience29@gmail.com         CLICK ON ADD SECRET
HEROKU_API_KEY          <dont keep it public>                   CLICK ON ADD SECRET
HEROKU_APP_NAME         cicd-app123                             CLICK ON ADD SECRET



ONCE ALL THE SECRETS ARE ADDED 
Go to ACTION TAB -> workflow ->  RERUN JOB AND THEN SELECT EITHER BUILD ALL JOBS/BUILD FAILED JOBS(TOP RIGHT CORNER) .YOU WILL SEE BUILD SUCCESSFUL.

Congrats , application is deployed successfully to HEROKU

open heroku -> click on your app -> click on open app

https://cicd-app123.herokuapp.com/  -- heroku link to fetch the result.

****now once u again push the result again the workflow will be created and again the buld will happen











/*

Build Docker Image manually

i)docker build -t <image_name>:<tagname> .

***image name must be lower case
****u can give any name for project and tag

example:- docker build -t cicd_project:latest .

ii) to list docker iamge
docker images

iii) To run the built docker image
docker run -p <portno>:<portno> -e PORT =<portno> <docker iamge id>

examples:- docker run -p 8001:8001 -e PORT =8001 f3d62b5538b5

**normally in local all apps are tied to 5000

iv) to check the status of running docker container
docker ps

v)to stop docker conatiners
docker stop <container id>

*/
