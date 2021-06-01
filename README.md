# Tor Script

After a Cyber Security Awareness Training for company X, I thought about sharing the idea and even the script used to make it easier and available for everyone.

![header](static/header.png)

### Before doign anything, you can check here some Tor uses/users statistics 05/2021 


* By Users : 
![stats](https://metrics.torproject.org/userstats-relay-country.png?start=2021-03-03&end=2021-06-01&country=all&events=off)

* By Country : 
![stats](https://i.imgur.com/suySqtJ.png)

* By Relays : 
![stats](https://metrics.torproject.org/networksize.png?start=2021-03-03&end=2021-06-01)

* By Relays : 
![stats](https://metrics.torproject.org/bandwidth-flags.png?start=2021-03-03&end=2021-06-01)


## Starting with docker 

So, here we'll use a docker image with Tor installed on it. We

# Docker 

On docker I'm going to use alpine instead of Debian on docker for it's light weight.

## Configuring the image

starting with tor config file `torrc` / (`/etc/tor/torrc`)
```
    VirtualAddrNetwork 0.0.0.0/10
    AutomapHostsOnResolve 1
    DNSPort 0.0.0.0:53530
    SocksPort 0.0.0.0:9050
```
> you can change port 1962 to your own

![Config](https://i.imgur.com/wewtou6.png)

and now the `Dockerfile`

```
FROM alpine:latest
RUN apk update && apk add tor
COPY torrc /etc/tor/torrc
RUN chown -R tor /etc/tor
USER tor
ENTRYPOINT ["tor"]
CMD ["-f", "/etc/tor/torrc"]
```
![Dockerfile](https://i.imgur.com/PbplMVn.png)

* The containing of the folder should be :

![output](https://i.imgur.com/wFmH7Sv.png)


Now let's build and image : `docker build -t sofiane/tor .`

![Built](https://i.imgur.com/LNLGq6c.png)

Check the image `docker image ls | grep sofiane/tor

![check](https://i.imgur.com/FAmPRFo.png)

## Using the proxy

Start by running the docker image `docker run --rm --detach --name tor --publish 1962:1962 sofiane/tor`

![](https://i.imgur.com/Ub5Rljr.png)

Now let's test it out!

* Without Proxy : My Real IP 
![noproxy](https://i.imgur.com/MHLZKzv.png)
* With Proxy : a Tor exit
![proxy](https://i.imgur.com/po3SHo2.png)
