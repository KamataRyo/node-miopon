# node-miopon
![node-miopon](icon.png)

[![Build Status](https://travis-ci.org/KamataRyo/node-miopon.svg?branch=master)](https://travis-ci.org/KamataRyo/node-miopon)
[![npm version](https://badge.fury.io/js/node-miopon.svg)](https://badge.fury.io/js/node-miopon)
![dependencies](https://david-dm.org/kamataryo/node-miopon.svg)
[![Code Climate](https://codeclimate.com/github/KamataRyo/node-miopon/badges/gpa.svg)](https://codeclimate.com/github/KamataRyo/node-miopon)

[みおぽんAPI](https://www.iijmio.jp/hdd/coupon/mioponapi.jsp)のNodejs ラッパーです。
oAuthとAPIへのアクセスをラップしています。



## install
`npm install node-miopon`

## usage
`miopon = require 'node-miopon'`

`coupon = new miopon.Coupon`



## 例

### 電話番号'09000000000'のクーポンをオンにする
    client_id = 'xxxxxxxxxxxxxxxxxxx'
    access_token = 'yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy'
    coupon = new Coupon()

    # request all the phone number list at first
    coupon.inform {
        client_id
        access_token
        success: ({information}) ->

            # filter and create query
            query = miopon.utility.querify {
                information
                couponUse: 'on'
                filter: '09000000000'
            }

            # request to turn the coupon on
            coupon.turn {
                client_id
                access_token
                query
                success: ->
                    console.log 'success!'
                failure: (err) ->
                    console.log err
            }
    }




## APIs

### oAuth

- `coupon.oAuth` takes `{ mioID, mioPass, client_id, redirect_uri, success, failure }`.
- callback `success` will be called with `{client_id, access_token, expires_in}`.
- callback `failure`  will be called with a `error object`.

### inform

- `coupon.inform` takes `{client_id, access_token, success, failure}`.
- callback `success` will be called with `{information}`.
- callback `failure`  will be called with a `error object`.

### turn

- `coupon.turn` takes `{client_id, access_token, query, success, failure}`.
- callback `success` will be called with no argument.
- callback `failure`  will be called with a `error object`.

### querify

- `utility.querify` takes `{information, couponUse, filter}` and returns `query` synchronously.
- optional `couponUse` accepts..
    + `'on'` or something to be evaluated as `true`
    + `'off'` or something to be evaluated as `false`
- optional `filter` accepts array or string of phone number(s) and filter query if provided.
