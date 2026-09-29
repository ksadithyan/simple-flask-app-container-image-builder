# simple-flask-app-container-image-builder
 a simple app.py using py3 flask to a container image using dockerfile. This is just to learn how it works (ignore the ENV inside the Dockerfile. Remember it's a bad practice to set it like this)


1) clone the repo
2) make sure u have docker installed
3) docker build -t adithyan/my-app .   

Note:  -t is the name/tag and the '.' represent the Dockerfile in the current dir

4) docker run -p 5000:5000 adithyan/my-app:latest
5) in web browser try the following 
   1) localhost:5000
   2) localhost:5000/how-are-you
6) Successfully see the two messages -welcome and - I'm fine. How are you? 


MAJOR ISSUES: with the traditional docker builder
1) REDOWNLOADING PACKAGES EVERY BUILD
2) SECRETS LEAK INTO METADATA 
   1) env (data gets baked onto the build history)
   2) copy + rm (data gets baked onto the build history)
   3) --build-arg (data gets baked onto the build history)
   4) multi-stage builds (better but risky so not preferable)
3) ARCHITECTURE LOCK-IN
   1) bad fix - seperate build machine
   2) bad fix - qmeu emulation by hand
4) INDEPENDENT STAGES RUN SEQUENTIALLY