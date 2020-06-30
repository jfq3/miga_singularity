# Installing MiGA Web in Windows

After installing Docker Desktop, do the following:

Start Docker Desktop. As it starts, the whale icon will appear among the hidden icons revealed by clicking on the ^ tab in the lower right of your screen. The whale will spout while Docker is starting, which takes a little while. Once Docker is running, the spouting will stop and a message that Docker is running should appear next to the icon.

With Docker Desktop running, open a terminal. You may use either Windows PowerShell or Command Prompt. Open your favorite by entering the name in the search box in the lower left of your screen.  

Enter the following in the terminal window:

```
docker pull fyuan277/miga-web:v1.1
```
The above downloads the docker image to your computer. You should have to do it only once. 

To start Miga-Web, enter the following into the terminal window one line at a time:

```
docker run -p 9090:3000 -it fyuan277/miga-web:v1.1 /bin/bash 
cd miga-web/
export SECRET_KEY_BASE=`bundle exec rake secret`
bundle exec rails server -e production -b 0.0.0.0 -p 3000 Puma
```
Open your browser and enter http://localhost:9090/ into the address bar. MiGA should start in your browser window.

Next you need to create an account by entering your name, emal address and a password. Then log in using your email address and password. 

# Problems
- You have to create an account every time Miga-Web is restarted.
- If you use the same email address, it won't let you.
- Creating a new project, you are asked to enter a path, but only a name is accepted (alphanumeric and underscore).
- I don't know what you can actually do once you have it running. Is everything lost once you stop the instance? Turn off your computer?
- If I try to log in, the email and password boxes appear to be populated with my Miga Online info. This maybe because of how my browser identifies the site?
