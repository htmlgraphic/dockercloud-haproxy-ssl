## Docker, load balancer and SSL certificate creation via Stack file

Phew! After a long focused duration of coding, troubleshooting and testing, SUCCESS! I'd like to share a funcntional `stackfile` able to create a load balancing service using Docker Cloud with a HAProxy with automatic SSL Certificate renewal via Let’s Encrypt.
 
In this guide I hope to help you save time and energy understanding the few important settings needed to have a sound and working solution.

Once you have used the updated Docker Cloud Stack in this repo you will have a number of web services with a HAProxy Load Balancing in front of them, redirecting any HTTP (Port 80) requests to HTTPS (Port 443) and a valid certificate automatically renewed and managed via letsencrypt-docker container via a persistent data volume.

---
## Deploy to Docker Cloud
Create a [Docker Cloud](https://cloud.docker.com) account, add a   **Cloud provider**. Once complete, `one click` will create a functional instance.

[![Deploy to Docker Cloud](https://files.cloud.docker.com/images/deploy-to-dockercloud.svg)](https://cloud.docker.com/stack/deploy/)

---

An important setting to understand is the exclusion of any open ports on the system you have behind your proxy. HAProxy will map out these open ports and try to route incoming requests to them.

`EXCLUDE_PORTS=443,22`

An error you receive without **EXCLUDE_PORTS** ([more info](https://stackoverflow.com/questions/44684001/haproxy-504-timeout-to-apache))

```
HAProxy returns 504 Gateway Timeout, indicating that the backend did not respond in a timely fashion.
```


The following repos are used with in this stack file.

* [https://github.com/docker/dockercloud-haproxy](https://github.com/docker/dockercloud-haproxy)
* [https://github.com/ixc/letsencrypt-docker](https://github.com/ixc/letsencrypt-docker)


---

Certbot renewal process via Crobjob:

```
$ /etc/periodic/daily/certbot
```

If you need to manage multiple domains note the following syntax within the Stackfile ([more info](https://github.com/ixc/letsencrypt-docker/blob/master/bin/certbot.sh#L29)):
`DOMAINS=example.com,www.example.com|example.net,www.example.net`

Helpful tips about volume within Docker Cloud Stacks and how the `volumes` avaiable from the `letsencrypt` container are referenced: [https://docs.docker.com/docker-cloud/apps/stack-yaml-reference/#volumes](https://docs.docker.com/docker-cloud/apps/stack-yaml-reference/#volumes)


---

## Patches / Feedback Welcome!
[Send a message](https://github.com/htmlgraphic/dockerclould-haproxy-ssl/issues/new)
