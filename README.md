# docker-mean-brownfield
A guide to setting up your docker container on AWS using the MEAN stack

## Project Setup

- [ ] Create an account at [aws.amazon.com](https://aws.amazon.com/ "AWS"). Make sure you sign up for the Basic plan, as that is the free tier.
![Sign In to AWS](./assets/images/01-brownfield-aws-signup.png "Logo Title Text 1")
- [ ] Launch the management console
 ![Management Console](./assets/images/02-brownfield-aws-console.png "Logo Title Text 2")
- [ ] Head to IAM so you can create your first user
 ![Open IAM](./assets/images/03-brownfield-aws-iam.png "Logo Title Text 3")
- [ ] Click the `user` menu item on the left to open the user management dashboard
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
