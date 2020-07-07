# Installing MiGA-Web in Windows

After installing Docker Desktop, do the following:

With Docker Desktop running, open a terminal. You may use either Windows PowerShell or Command Prompt. Open your favorite by entering the name in the Windows search box in the lower left of your screen.  

Enter the following in the terminal window:

```
docker pull fyuan277/miga-web:v1.1
```
The above downloads the docker image to your computer. You should have to do it only once, *i.e.* the first time you use MiGA-Web. 

To start Miga-Web, enter the following into the terminal window one line at a time. You will have to do this each time you use MiGA-Web.

```
docker run -p 9090:3000 -it fyuan277/miga-web:v1.1 /bin/bash 
cd miga-web/
export SECRET_KEY_BASE=`bundle exec rake secret`
bundle exec rails server -e production -b 0.0.0.0 -p 3000 Puma
```
Open your browser and enter http://localhost:9090/ into the address bar. MiGA should start in your browser window.

Next you need to create an account by entering your name, email address and a password. Then log in using your email address and password. 

# Problems/Questions
- You have to create an account every time an instance of Miga-Web is started. You can logout and even close the browser, and then log back in as long as the same Docker instance is running.
- Creating a new project, you are asked to enter a path, but only a name is accepted (alphanumeric and underscore). It is not really a path, but a project name.
- Apparently everything is lost once you stop the instance, so you cannot save datasets. Results are not saved except by downloading everything before stopping the instance.
