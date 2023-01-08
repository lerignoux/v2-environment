# v2-environment
A small v2 demo environment.

This is for technical knowledge and information only. Ensure with your legislation before use.

## tldr
On server:
* Generate dhparams
* Generate certificates
* `docker-compose up -d`

On client: 
```
docker run --rm --name v2ray_client -it -v "~/v2-environment/v2/client.json:/etc/v2ray/config.json" -p 6666:6666 v2ray/official
```

## Setup
Setup assumes you are on a machine with Docker installed.
On windows you may need to adapt the volumes mount addresses to match Windows file system

### Setup a host machine:
Non-exhaustive VPS providers list
* AWS
* Google Cloud
* OVH
* Azure
* scaleway
* cubecloud
* gigs
* vultr
* linode
* sugarhosts.com
* bandwagonhost
* hostshare
* Enoctus

### Optional: Setup a load balancer:
For isntance in AWS CloudFront Origin and Distribution pointing to your VPS
You can then use the domain name in your `client.json` configuration

### Checkout the project on server and client side
we will assume project has been cloned in your user folder, adapt accordingly otherwise
```
git clone https://github.com/lerignoux/v2-environment.git
```

### Generate certificates:
See with your Cloud provider.

Example with CloudFlare:
Fill in your cloudflare api key in `ACME/cloudflare.ini`
```
docker run -it --rm --name certbot_<domain> -v ~/v2-environment/certificates:/etc/letsencrypt -v ~/v2-environment/ACME/cloudflare.ini:/etc/cloudflare.ini certbot/dns-cloudflare certonly -n -m <your email> --agree-tos --dns-cloudflare --dns-cloudflare-credentials /etc/cloudflare.ini -d <domain>
```
The certificates should be generated in `certificates/`

### Generate dhparams file
See [stackoverflow](https://techoverflow.net/2019/04/02/how-to-generate-diffie-hellman-dh-parameters-using-openssl/) for details

```
openssl dhparam -out dhparams.pem 4096
```

### Start the Server
Start the server:
```
docker-compose up -d
```

At this point going to your server url/cloud front url should show you a simple webpage

### Start the client:
docker run --rm --name v2ray_client -it -v "~/v2-environment/v2/client.json:/etc/v2ray/config.json" -p 6666:6666 v2ray/official

You now have a socks5 proxy in your container that would forward all requests to your server.
Address: `localhost:6666`
If you need a http proxy, adding a privoxy entry for instance should be fairly straightforward

### Contribution:
If you notice anything weird, not working or suggest an improvement feel free to send a Merge Request.