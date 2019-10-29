---
layout: single
title: 'Gitlab Runner Registration x509 Certificate Error'
description: 'This step by step post walks you through how to fix the x509 Certificate error that can occur when registering a Runner.'
date: '2019-09-06'
classes: wide

header:
  overlay_color: "#5e616c"
  overlay_image: /assets/images/jekyll-island.jpg
---

--------------------------------------------

GitLab® is a open-source licensed web-based DevOps platform that provides a Git-repository manager wiki combined with  issue-tracking, CI/CD pipeline features, developed by GitLab Inc.

A common issue when working with Gitlab initially is configuration of the environment as related to security.  Security is great but when it prevents you from running a pipeline it needs to be fixed. Use the following steps to create a new certificate for gitlab-runner or to replace it if it has expired.

### Prerequisites

In order to proceed with this guide it is necessary to have a working installation of Gitlab running on either bare metal or a docker container. 

## Step 1 Create or Replace x509 Certificate

Login to the server where Gitlab is installed and become root as the /etc/gitlab-runner directory is owned by root. 

```
sudo -i
```

Next, cd to the /etc/gitlab-runner directory. 

```
cd /etc/gitlab-runner
```

If you are just getting started you will want to run the following command to that creates a folder and a certificate.  

```
mkdir -p /etc/gitlab-runner/certs && openssl s_client -connect gitlab.example.io:443 -showcerts < /dev/null | openssl x509 -outform PEM > /etc/gitlab-runner/certs/gitlab.example.io.crt
```

Note: You will need to change the domain name in the above commands.  

If you already have a certs folder and the certificate is now invalid. Run the following commands first.

```
cd /certs
rm -R *
```
This will remove the invalid certificates.  To replace run the following command.

```
openssl s_client -connect gitlab.example.io:443 -showcerts < /dev/null | openssl x509 -outform PEM > /etc/gitlab-runner/certs/gitlab.example.io.crt
```

Finally, after creating the new certificate export it to PEM format.

```
openssl s_client -connect gitlab.example.io:443 -showcerts < /dev/null | openssl x509 -outform PEM > ca.crt
```

## Step 2 Validate the certificate

To validate the certificates on you server run the following command. 

```
openssl s_client -showcerts -connect gitlab.example.io:443 </dev/null
```

## Conclusion

If the initial issue was not being able to register a runner for a pipeline, you should now be able to register a runner for the pipeline. 
