
* To connect to my EC2 instance, I was required to SSH into it. I had challenges doing this because I could not suucessfully convert my key pair from .pem to .ppk and use putty SSH server, so I followed 
the alternative steps below;

- Download git
- Add Gitbash as a different profile on windows terminal
- Add the exe file to the file path on windows terminal
- cd to the actual location of the key pair file
- chmod on the key pair file
- Edit the inbound rules for the security groupmy instance is running on 
- run the connection command from my aws console to connect to the instance


* Another way to retrieve your Public IP address, other than to check it in AWS Web console, is to use following command:
- curl -s http://169.254.169.254/latest/meta-data/public-ipv4
If this doesnt work read on metadata and IMDV2
TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` 
curl -H "X-aws-ec2-metadata-token: $TOKEN" -v http://169.254.169.254/latest/meta-data/public-ipv4

