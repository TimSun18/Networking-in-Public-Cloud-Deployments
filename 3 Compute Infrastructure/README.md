## Task 1 - Configuring Interfaces

In prior labs, you've used the `src` and `commands` parameters of the `config` modules.  While you can deploy any commands using `src`, `commands` by itself are limited to global configurations.

This task introduces the `parents` parameter when using the `config` module to configure commands that have hierarchy.

This task creates an EC2 with ubuntu and then installs apache2 web server. After the initialization, the server grabs an html file from the S3 bucket. The html file (index.html) is a static website containing an image pointing to S3 bucket as well 
#### Preparation
