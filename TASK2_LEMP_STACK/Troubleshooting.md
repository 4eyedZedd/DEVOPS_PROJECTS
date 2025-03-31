

## .PHP Config not working

I experienced challenges while trying to run the urls
``` 
http://13.60.15.18/index.php and http://ec2-13-60-15-18.eu-north-1.compute.amazonaws.com/index.php
```
and I was getting a '502' bad gateway error. While troubleshooting, I followed thefollowing steps

* Review the configuration file and prioritize index.php files over index.html
* Changed the servername on the configuration file to my instance public ipv4 and DNS name
* I unlinked sites-available default file again.
* I reloaded nginx and php-fpm again.
* I pinged the server to know if it was alive
* 

Although all these did not solve the problem, but it gave me an insight into troubleshooting issues more.

* I then escalated to my tutor and we discovered that the error was because of a version problem with the configuration file for nginx.

I had downloaded a php-fpm version 8.3 and while the configuration file I used had a version 8.1, after I corrected it, the script ran as expected.