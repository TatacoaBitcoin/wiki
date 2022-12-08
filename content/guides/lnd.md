---
title: 'LND'
metaTitle: 'Guías - LND'
metaDescription: 'How-to guides'
---

# lncli

Control plane for your Lightning Network Daemon (lnd)

## version

btcpay

    $ lncli --version
    lncli version 0.14.1-beta commit=basedon-v0.14.1-beta-fresh-btcpay
    $ lncli --macaroonpath data/admin.macaroon version
    {
        "lncli": {
            "commit": "basedon-v0.14.1-beta-fresh-btcpay",
            "commit_hash": "94693c31b77ec08777593de287fef50cd1670af0",
            "version": "0.14.1-beta",
            "app_major": 0,
            "app_minor": 14,
            "app_patch": 1,
            "app_pre_release": "beta",
            "build_tags": [
                "signrpc",
                "walletrpc",
                "chainrpc",
                "invoicesrpc",
                "routerrpc"
            ],
            "go_version": "go1.17.1"
        },
        "lnd": {
            "commit": "basedon-v0.14.1-beta-fresh-btcpay",
            "commit_hash": "94693c31b77ec08777593de287fef50cd1670af0",
            "version": "0.14.1-beta",
            "app_major": 0,
            "app_minor": 14,
            "app_patch": 1,
            "app_pre_release": "beta",
            "build_tags": [
                "signrpc",
                "walletrpc",
                "chainrpc",
                "invoicesrpc",
                "routerrpc"
            ],
            "go_version": "go1.17.1"
        }
    }

polar

    $ lncli --version
    lncli version 0.12.1-beta commit=v0.12.1-beta
    $ lncli version
    {
        "lncli": {
            "commit": "v0.12.1-beta",
            "commit_hash": "d233f61383f2f950aa06f5b09da5b0e78e784fae",
            "version": "0.12.1-beta",
            "app_major": 0,
            "app_minor": 12,
            "app_patch": 1,
            "app_pre_release": "beta",
            "build_tags": [
                "autopilotrpc",
                "signrpc",
                "walletrpc",
                "chainrpc",
                "invoicesrpc",
                "watchtowerrpc"
            ],
            "go_version": "go1.15.7"
        },
        "lnd": {
            "commit": "v0.12.1-beta",
            "commit_hash": "d233f61383f2f950aa06f5b09da5b0e78e784fae",
            "version": "0.12.1-beta",
            "app_major": 0,
            "app_minor": 12,
            "app_patch": 1,
            "app_pre_release": "beta",
            "build_tags": [
                "autopilotrpc",
                "signrpc",
                "walletrpc",
                "chainrpc",
                "invoicesrpc",
                "watchtowerrpc"
            ],
            "go_version": "go1.15.7"
        }
    }

## Wallet

polar

    $ lncli walletbalance
    {
        "total_balance": "20484726",
        "confirmed_balance": "20484726",
        "unconfirmed_balance": "0"
    }

## Invoices

polar

    $ lncli listinvoices
    $ lncli addinvoice --memo "lncli test" --amt 20
    {
        "r_hash": "7a1eb4a95797f44245923c3b1d349b4ece3d2502a939409d595670aa6edab4b1",
        "payment_request": "lnbcrt200n1ps7kjzepp50g0tf22hjl6yy3vj8sa36dymfm8r6fgz4yu5p82e2ec25mk6kjcsdqsd3hxxmrfyp6x2um5cqzpgsp5unckz5p43akcg4km7tmdjzcfwsu84xnplr8ryfvpcf0vxs9jsvds9qyyssqzlfkhvq58szcnnalzzgepu0m2ur59pue9l9zfyvz3lnfdvz5vs7kymyttzrjlc47q80u7cs4j7px3m789q7x4d8xntcxaamaupwmgkqqhl7984",
        "add_index": "5",
        "payment_addr": "e4f16150358f6d8456dbf2f6d90b0974387a9a61f8ce322581c25ec340b2831b"
    }
    $ lncli lookupinvoice 7a1eb4a95797f44245923c3b1d349b4ece3d2502a939409d595670aa6edab4b1
    {
        "memo": "lncli test",
        "r_preimage": "58184e684f47013b23f8ac8774095b757f537953d6d4a0df400439288517c9b7",
        "r_hash": "7a1eb4a95797f44245923c3b1d349b4ece3d2502a939409d595670aa6edab4b1",
        "value": "20",
        "value_msat": "20000",
        "settled": false,
        "creation_date": "1642809433",
        "settle_date": "0",
        "payment_request": "lnbcrt200n1ps7kjzepp50g0tf22hjl6yy3vj8sa36dymfm8r6fgz4yu5p82e2ec25mk6kjcsdqsd3hxxmrfyp6x2um5cqzpgsp5unckz5p43akcg4km7tmdjzcfwsu84xnplr8ryfvpcf0vxs9jsvds9qyyssqzlfkhvq58szcnnalzzgepu0m2ur59pue9l9zfyvz3lnfdvz5vs7kymyttzrjlc47q80u7cs4j7px3m789q7x4d8xntcxaamaupwmgkqqhl7984",
        "description_hash": null,
        "expiry": "3600",
        "fallback_addr": "",
        "cltv_expiry": "40",
        "route_hints": [
        ],
        "private": false,
        "add_index": "5",
        "settle_index": "0",
        "amt_paid": "0",
        "amt_paid_sat": "0",
        "amt_paid_msat": "0",
        "state": "OPEN",
        "htlcs": [
        ],
        "features": {
            "9": {
                "name": "tlv-onion",
                "is_required": false,
                "is_known": true
            },
            "14": {
                "name": "payment-addr",
                "is_required": true,
                "is_known": true
            },
            "17": {
                "name": "multi-path-payments",
                "is_required": false,
                "is_known": true
            }
        },
        "is_keysend": false,
        "payment_addr": "e4f16150358f6d8456dbf2f6d90b0974387a9a61f8ce322581c25ec340b2831b"
    }
    $ lncli walletbalance
    {
        "total_balance": "20484726",
        "confirmed_balance": "20484726",
        "unconfirmed_balance": "0"
    }

## Peers

lncli --macaroonpath data/admin.macaroon listpeers
{
"peers": [
{
"pub_key": "021744d86987a91958461117cd9e7c0e3160f7b86de11f5998018f4b4984a5c330",
"address": "54.194.246.117:9735",
"bytes_sent": "3214488",
"bytes_recv": "128237726",
"sat_sent": "0",
"sat_recv": "0",
"inbound": false,
"ping_time": "93869",
"sync_type": "ACTIVE_SYNC",
"features": {
"0": {
"name": "data-loss-protect",
"is_required": true,
"is_known": true
},
"5": {
"name": "upfront-shutdown-script",
"is_required": false,
"is_known": true
},
"7": {
"name": "gossip-queries",
"is_required": false,
"is_known": true
},
"9": {
"name": "tlv-onion",
"is_required": false,
"is_known": true
},
"12": {
"name": "static-remote-key",
"is_required": true,
"is_known": true
},
"14": {
"name": "payment-addr",
"is_required": true,
"is_known": true
},
"17": {
"name": "multi-path-payments",
"is_required": false,
"is_known": true
},
"23": {
"name": "anchors-zero-fee-htlc-tx",
"is_required": false,
"is_known": true
},
"31": {
"name": "amp",
"is_required": false,
"is_known": true
},
"45": {
"name": "explicit-commitment-type",
"is_required": false,
"is_known": true
},
"2023": {
"name": "script-enforced-lease",
"is_required": false,
"is_known": true
}
},
"errors": [
],
"flap_count": 1,
"last_flap_ns": "1642463349444290051",
"last_ping_payload": "04e0ff271bae425ebd43bb68a17c206c1ea955823479dd07022600000000000000000000037eef3ef2438b72d806fb8a201d3527d9cbcc88f90c716f1c7b4398ab62ba95e24eeb6180900a170d1cc404"
},
{
"pub_key": "036adc8843c916e6eb30e18dad85832f93580d26d164c070c607e02d8a2cc33f2a",
"address": "212.83.133.135:4027",
"bytes_sent": "33934141",
"bytes_recv": "169617197",
"sat_sent": "0",
"sat_recv": "0",
"inbound": false,
"ping_time": "92789",
"sync_type": "ACTIVE_SYNC",
"features": {
"0": {
"name": "data-loss-protect",
"is_required": true,
"is_known": true
},
"5": {
"name": "upfront-shutdown-script",
"is_required": false,
"is_known": true
},
"7": {
"name": "gossip-queries",
"is_required": false,
"is_known": true
},
"9": {
"name": "tlv-onion",
"is_required": false,
"is_known": true
},
"12": {
"name": "static-remote-key",
"is_required": true,
"is_known": true
},
"14": {
"name": "payment-addr",
"is_required": true,
"is_known": true
},
"17": {
"name": "multi-path-payments",
"is_required": false,
"is_known": true
}
},
"errors": [
],
"flap_count": 1,
"last_flap_ns": "1642463349479626666",
"last_ping_payload": null
},
{
"pub_key": "0255c642e85f7ba11d9b6edfe72e78d4dff7637a0b9ad208f613c78687ca79e054",
"address": "185.150.160.209:4020",
"bytes_sent": "11260571",
"bytes_recv": "125986388",
"sat_sent": "0",
"sat_recv": "0",
"inbound": false,
"ping_time": "89057",
"sync_type": "ACTIVE_SYNC",
"features": {
"0": {
"name": "data-loss-protect",
"is_required": true,
"is_known": true
},
"5": {
"name": "upfront-shutdown-script",
"is_required": false,
"is_known": true
},
"7": {
"name": "gossip-queries",
"is_required": false,
"is_known": true
},
"9": {
"name": "tlv-onion",
"is_required": false,
"is_known": true
},
"12": {
"name": "static-remote-key",
"is_required": true,
"is_known": true
},
"14": {
"name": "payment-addr",
"is_required": true,
"is_known": true
},
"17": {
"name": "multi-path-payments",
"is_required": false,
"is_known": true
},
"23": {
"name": "anchors-zero-fee-htlc-tx",
"is_required": false,
"is_known": true
},
"31": {
"name": "amp",
"is_required": false,
"is_known": true
}
},
"errors": [
],
"flap_count": 1,
"last_flap_ns": "1642463349655597639",
"last_ping_payload": null
}
]
}

## Channels

    $ lncli --macaroonpath data/admin.macaroon listchannels
    {
        "channels": [
        ]
    }
