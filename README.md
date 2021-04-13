# haproxy-crl-demo
Demo on how to configure Certificate revocation list for HaProxy

## How to run HaProxy on localhost machine using docker ?

``` shell
docker run --net host --rm -v ./crl_test:/crl_test haproxy:2.2.10-alpine -f /crl_test/h.cfg
```

## How to test HaProxy ?

### With valid certificate

``` shell
curl --location --request GET  --cert './Intermediate/issued/valid.crt:secret' --key ./Intermediate/private/valid.key --cacert ./Intermediate/issued/domain.com.crt --resolve domain.com:443:127.0.0.1 https://domain.com

```

### With revoked certificate

``` shell
curl --location --request GET  --cert './Intermediate/issued/revoked.crt:secret' --key ./Intermediate/private/revoked.key --cacert ./Intermediate/issued/domain.com.crt --resolve domain.com:443:127.0.0.1 https://domain.com
```

## The purpose of this repository

This repo was created in order to be able to reproduce issue that I had when revoking leaf certificate. 
It turned out that if I have chain of certs like follows `RootCA -> Intermediate -> LeafCert` and I will generate LeafCert then HaProxy will reject any client certificate, even valid ones. The reason is that HaProxy in version at least up to 2.2.10 require to have CRL section for each CA. Because of that when you look at CRL file for Intermediate certificate you will see two CRL sections, one for Intermediate and another one for RootCA. For more details refer to this bug : https://github.com/haproxy/haproxy/issues/1201.  
