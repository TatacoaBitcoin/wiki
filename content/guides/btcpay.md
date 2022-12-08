---
title: 'BTCPay Server'
metaTitle: 'Guías - BTCPay Server'
metaDescription: 'How-to guides'
---

# Deployment of BTCPay Server through LunaNode Web-Wizard

1. Create account in LunaNode and generate an api key.

2. Go to https://launchbtcpay.lunanode.com/ and provide auth info.

3. Set hostname.

4. Set server info and ssh key.

# LND GRPC

More details

## Get cert and macaroon from server

    $ ssh -i path_to/sshkey.priv user@8.8.8.8
    # docker exec btcpayserver_lnd_bitcoin base64 -w0 /data/tls.cert
    # docker exec btcpayserver_lnd_bitcoin base64 -w0 /data/admin.macaroon

## delete the old certificate and key and have LND generate new ones

## Check SSL certificate

    $ ssh -i path_to/sshkey.priv user@8.8.8.8
    # docker exec -it letsencrypt-nginx-proxy-companion /app/cert_status

No se necesita certificado, https://github.com/alexbosworth/ln-service/issues/70

## Main Issue

Using lightning@5.3.4

Trying to:

    try {
    	payment = await payViaPaymentRequest({
    	  	lnd,
      		request: paymentData.request,
    	});
    } catch (e) {
    	console.error(e);
    	return res.status(500).send({ error: 'Payment failed' });
    }

Return error:

    [
      503,
      'UnexpectedPaymentError',
      {
        err: Error: 12 UNIMPLEMENTED:
            at Object.callErrorFromStatus (/home/max/workspace/tatacoa-api/node_modules/@grpc/grpc-js/build/src/call.js:31:26)
            at Object.onReceiveStatus (/home/max/workspace/tatacoa-api/node_modules/@grpc/grpc-js/build/src/client.js:331:49)
            at Object.onReceiveStatus (/home/max/workspace/tatacoa-api/node_modules/@grpc/grpc-js/build/src/client-interceptors.js:299:181)
            at /home/max/workspace/tatacoa-api/node_modules/@grpc/grpc-js/build/src/call-stream.js:160:78
            at processTicksAndRejections (node:internal/process/task_queues:78:11) {
          code: 12,
          details: '',
          metadata: [Metadata]
        }
      }
    ]

## Access node

    ssh -i ~/tatacoa/Credenciales/ssssh ubuntu@serverip

Check lnd logs

    docker logs --tail 100 btcpayserver_lnd_bitcoin

ssh into container

    docker exec -it btcpayserver_lnd_bitcoin /bin/bash

print default btcpay config from the container

    cat data/lnd.conf

print default btcpay config from btcpay docker compose

    cat btcpayserver-docker/docker-compose-generator/docker-fragments/bitcoin-lnd.yml

update lnd configs in btcpay

    sudo su -
    vi btcpayserver-docker/docker-compose-generator/docker-fragments/opt-lnd-config.custom.yml


    version: "3"
    services:
      lnd_bitcoin:
        environment:
          LND_EXTRA_ARGS: |
            allow-circular-route=true

    export BTCPAYGEN_ADDITIONAL_FRAGMENTS="$BTCPAYGEN_ADDITIONAL_FRAGMENTS;opt-lnd-config.custom"
    cd btcpayserver-docker
    . ./btcpay-setup.sh -i

https://docs.btcpayserver.org/Docker/#how-can-i-customize-the-generated-docker-compose-file

https://docs.btcpayserver.org/FAQ/LightningNetwork/#how-to-edit-lndconf
