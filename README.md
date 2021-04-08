# haproxy-crl-demo
Demo on how to configure Certificate revocation list for HaProxy

## How to run it ?

``` shell
docker run --net host --rm -v /home/tomek/Desktop/crl_test:/crl_test haproxy:2.2.10-alpine -f /crl_test/h.cfg
```
