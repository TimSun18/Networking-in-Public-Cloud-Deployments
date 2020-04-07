## Introduction

This task creates an EC2 with ubuntu AMI and then installs apache2 web server. After the initialization, the server grabs an html file from the S3 bucket. The html file (index.html) is a static website containing an image pointing to S3 bucket as well. 

#### Preparation

Create an S3 bucket and upload the index.html and an image to it.

![Image of S3 Bucket](https://octodex.github.com/images/yaktocat.png)
