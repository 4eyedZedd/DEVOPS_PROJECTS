
## Installing MongoDB 

The guide requested to use the command below to install MongoDB, but it is already deprecated as it is an older version;

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```

To by pass this, I visited the official mongoDB [website](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/) and followed the steps listed under the *'Install MongoDB Community Edition'* subtopic.

