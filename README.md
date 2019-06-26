### companyNews App on Vagrant + Docker-swarm

#### Tool stacks
* Vagrant
* Docker
* Docker swarm for orchestration
* OS: Centos/7

#### Prerequisite
Clone the repo on local system from where you want to bring-up the setup. Install the below packages:
* <b>Docker-ce:</b> for image creation 
* <b>Vagrant:</b>

#### Instruction
First lets create the docker image for our app and webserver.<br>

* Appserver docker image creation
```
]# cd thwrk-assigment/appserver
]# docker build -t saviovettoor/thwrkappserver .

Test the image is working as exepected buy running a sample container of the image:

]#docker run --rm --name thwrkappserver -p 8080:8080 saviovettoor/thwrkappserver
Now the you can access the app using http://<hosip>:8080
```

* Webserver docker image creation
```
]#cd thwrk-assigment/webserver
]# docker build -t saviovettoor/thwrkwebserver .

Test the image is working as exepected buy running a sample container of the image:

]#docker run --rm --name thwrkappserver -p 80:80 saviovettoor/thwrkwebserver
Now the you can access the app using http://<hosip>
```

* Push image to the docker-hub
```
Firt login to th hub from where you are going to push the image
]# docker login
PROVIDE YOUR USER NAME AND PASSOWORD

]# docker push saviovettoor/thwrkappserver
]# docker push saviovettoor/thwrkwebserver

NOTE: WHEN YOU ARE TESTING MAKE SURE YOU HAVE IMAGE NAME WITH YOUR HUB USERNAME AND REPO NAME.
```

```
NOW lets bring up the app:
1. Move on to the main folder "thwrk-assigment"
2. vagrant up

This will bringup two machines with dockerswarm cluster and your app.

NOTE: in the setup we have use birectional sync of file(swarm token) to bring up the cluster, some this can fail based on vagrant version and virtualbox version which you are using, there already a bug on this.IF failed what need to be done to bring up the app [link](https://github.com/saviovettoor/thwrk-assigment/wiki/On-fail!!!!)
```
Now let put a local host entry in your local and access the app.<br>
```
Windows: c:\Windows\System32\Drivers\etc\hosts<br>
Linux: /etc/hosts<br>

10.0.8.20 www.companynews.com
```
Now you will be able to access the app over http and https: www.companynews.com

#### MISC
---------
```
How self signed certificated (link)[https://github.com/saviovettoor/DevOps-wiki/wiki/self-signed-certificates]
If you have a valid domain and no need a self signed one you go can goahead with ssl certificate providers paid one or free on the Let's  Encrypt(https://github.com/saviovettoor/DevOps-wiki/wiki/Let%E2%80%99s-Encrypt-SSL-Certificates-using-Standalone-Manual-Mode)
```

### On Production
To make production ready setup please have a look [link](https://github.com/saviovettoor/thwrk-assigment/wiki/On-production)
