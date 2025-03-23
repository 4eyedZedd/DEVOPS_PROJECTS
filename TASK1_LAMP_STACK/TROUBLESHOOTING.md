
## USING GITBASH TO SSH INTO YOUR EC2 INSTANCE SETUP.

* To connect to my EC2 instance, I was required to SSH into it. I had challenges doing this because I could not suucessfully convert my key pair from .pem to .ppk and use putty SSH server, so I followed 
the alternative steps below;

    - Download git and install the package
    - Add Gitbash as a different profile on windows terminal
    - Add the exe file to the file path on windows terminal
    - cd to the actual location of the key pair file. `cd /c/users/DELL/downloads`
    - chmod on the key pair file. `sudo chmod 0400 <private-key-name>.pem`
    - Edit the inbound rules for the security group my instance is running on. Permit inbound HTTP traffic from anywhere i.e 0.0.0.0/0
    - Connect to the instance by running the command: `ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>`. Keep in mind that your public IPv4 address changes every time you restart your instance.


## ALTERNATE WAY TO RETRIEVE YOUR PUBLIC IP ADDRESS 


* Another way to retrieve your Public IP address, other than to check it in AWS Web console, is to use following command:

>`curl -s http://169.254.169.254/latest/meta-data/public-ipv4 
If this doesnt work read on metadata and IMDV2`

>`TOKEN=curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` 

>`curl -H "X-aws-ec2-metadata-token: $TOKEN" -v http://169.254.169.254/latest/meta-data/public-ipv4`

* See reference for retrieving your AWS instance [here](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html)

