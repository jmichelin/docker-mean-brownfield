# docker-mean-brownfield
A guide to setting up your docker container on AWS using the MEAN stack

## Prerequisite 
- [ ] Install Docker
- [ ] 

## Project Setup

- [ ] Create an account at [aws.amazon.com](https://aws.amazon.com/ "AWS"). Make sure you sign up for the Basic plan, as that is the free tier.

![Sign In to AWS](./assets/images/01-brownfield-aws-signup.png "Logo Title Text 1")

- [ ] Launch the management console

 ![Management Console](./assets/images/02-brownfield-aws-console.png "Logo Title Text 2")

- [ ] Head to IAM so you can create your first user

 ![Open IAM](./assets/images/03-brownfield-aws-iam.png "Logo Title Text 3")

- [ ] Click the `users` menu item on the left to open the user management dashboard

 ![Create IAM user](./assets/images/04-brownfield-aws-iam-user.png "Logo Title Text 4")

- [ ] Click the `Add user` button

![Create IAM user](./assets/images/05-brownfield-aws-iam-user-add.png "Logo Title Text 5")

- [ ] Enter a user name i.e. meanjs and check Programmatic access and click `Next: Permissions`

![Create IAM user](./assets/images/06-brownfield-aws-user-userinfo.png "Logo Title Text 6")

 - [ ] Create a group for the user i.e. dockerMeanApp

![Create IAM user](./assets/images/07-brownfield-aws-user-groupinfo.png "Logo Title Text 6")

 - [ ] Set the policy for the group, check `AdministratorAccess` and click `Create group`

![Create IAM user](./assets/images/08-brownfield-aws-group-security.png "Logo Title Text 6")

![Create IAM user](./assets/images/09-brownfield-aws-group-created.png "Logo Title Text 6")

- [ ] Review user and group. Verify `Programmatic access` and click `Create user` to continue

![Create IAM user](./assets/images/10-brownfield-aws-review.png "Logo Title Text 6")

- [ ] Download the credentials and click `close`

![Create IAM user](./assets/images/11-brownfield-aws-download-credentials.png "Logo Title Text 6")

- [ ] Save credentials to your development machine if it's the first time execute the line below, otherwise just add them
```bash
mkdir ~/.aws && echo -e '[default]\naws_access_key_id = [access key from file]\naws_secret_access_key = [secret key from file]' > ~/.aws/credentials
```

- [ ] Get vpc-id
![Create IAM user](./assets/images/12-brownfield-aws-vpcid-1.png "Logo Title Text 6")
![Create IAM user](./assets/images/13-brownfield-aws-vpcid-2.png "Logo Title Text 6")
![Create IAM user](./assets/images/14-brownfield-aws-vpcid-3.png "Logo Title Text 6")

We now have everything to fire up a new EC2 instance from a shell on your computer. (use `aws ec2 describe-subnets
` to find your available subnets)

```bash
$ docker-machine create --driver amazonec2  --amazonec2-vpc-id [your-vpc-id] --amazonec2-zone a aws-mean
Running pre-create checks...
Creating machine...
(aws01) Launching instance...
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env aws-mean
```

Once everything has settled we must instruct docker to execute on our new EC2.
```bash
$ eval $(docker-machine env aws-mean)
```

#launching mongo

First we must pull the image down and run it on a detached container. The `--name` command sets the instance name. 
The `-d` flag runs it in detached mode, and the `mongo` tells docker what image we want to use. 

```bash
$ docker run --name mymongodb -d mongo
```

#grab and launch the mean stack in a docker

```bash
docker run -i -t --name mymeanjs --link mymongodb:db_1 -p 80:3000 maccam912/meanjs:latest bash
```



Breakdown of above command:
1. -i = Keep STDIN open even if not attached (http://stackoverflow.com/questions/22272401/what-does-it-mean-to-attach-a-tty-std-in-out-to-dockers-or-lxc)
2. -t = Allocate a pseudo-tty
3. --name = names this instance
4. --link = Add link to another container (<name or id>:alias or <name or id>)
5. -p = expose instance port and set up port mapping hostPort:containerPort
6. maccam912/meanjs:latest is the mean stack image

You should see something like `root@2134h4238241:/#`
Type `ls` and you will see a `Development` folder, this is where we will hold our source code.
```bash
$ cd Development #change to Development directory
$ git clone https://github.com/meanjs/mean.git meanjs #grab the latest meanjs stack
$ cd meanjs
$ ls #verify the clone
$ npm install -g n #install n to manage node versions https://github.com/tj/n
$ npm i
$ bower install --allow-root
$ npm i grunt # might not need this
```

# Security on our instance
- [ ] Open EC2 console. 
- [ ] Find your instance. In our case it will be named aws-mean.
- [ ] Scroll to the right and you will see `Security Groups` click on it, in our case it is `docker-machine` 

![Create IAM user](./assets/images/15-brownfield-aws-ec2-security-1.png "Logo Title Text 6")

- [ ] Select the inbound tab. We are going to be adding HTTP to the rules, along side the SSH that is already there.
- [ ] Click edit and then Add
![Create IAM user](./assets/images/16-brownfield-aws-ec2-security-2.png "Logo Title Text 6")
![Create IAM user](./assets/images/17-brownfield-aws-ec2-security-3.png "Logo Title Text 6")

- [ ] Find the public address and head there in your browser. 

![Create IAM user](./assets/images/18-brownfield-aws-get-url.png "Logo Title Text 6")

If you see the splash page you're ready to get started!



