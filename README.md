# Introduction

* [Gitee仓库](https://gitee.com/samcoding/iOSExample.git)
* [Xcode Help](https://help.apple.com/xcode/mac/current/#/)
* [Apple Open Source](https://opensource.apple.com/source/)
* [Apple Open Source](https://opensource.apple.com)


# App Store Connect API

> 自动连接开发者的网站。

## Create API and Tokens

[Create API and Tokens](https://developer.apple.com/documentation/appstoreconnectapi/creating_api_keys_for_app_store_connect_api)

`usernames` 
`passwords`

`https://appstoreconnect.apple.com/login` -> 用户和访问 -> 密钥

Issuer ID : d404cae3-cf5b-45f3-bd83-b8559ba2b8d4
密钥 ID : M24UJ5Z62W

## Generating Tokens for API Requests

```json
// Header

{
	"alg" : "ES256",// dAll JWTs for App Store Connect API must be signed with ES256 encryption 
	"kid" : "M24UJ5Z62W",// Your private key ID from App Store Connect (Ex: 2X9R4HXF34)
    "typ": "JWT"
}

// JWT Payload
{
	"iss":"d404cae3-cf5b-45f3-bd83-b8559ba2b8d4",// Issuer ID: Your issuer ID from the API Keys page in App Store Connect (Ex: 57246542-96fe-1a63-e053-0824d011072a)
	"exp":"1611300840",// Expiration Time(秒): The token's expiration time, in Unix epoch time; tokens that expire more than 20 minutes in the future are not valid (Ex: 1528408800)
	"aud":"appstoreconnect-v1"// Audience : appstoreconnect-v1
}
```

`https://api.appstoreconnect.apple.com/v1/apps`



Authorization: Bearer eyJhbGciOiJIUzI1NiIsImtpZCI6Ik0yNFVKNVo2MlciLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJkNDA0Y2FlMy1jZjViLTQ1ZjMtYmQ4My1iODU1OWJhMmI4ZDQiLCJleHAiOiIxNjExMzAwODQwIiwiYXVkIjoiYXBwc3RvcmVjb25uZWN0LXYxIn0.WmnRwdW7M0vgWsX-sH8vvj3C0oBgqiBkNbxqX7-ZmZ8

eyJraWQiOiJNMjRVSjVaNjJXIiwiYWxnIjoiRVMyNTYifQ.eyJpc3MiOiJkNDA0Y2FlMy1jZjViLTQ1ZjMtYmQ4My1iODU1OWJhMmI4ZDQiLCJleHAiOjE2MTEzMDM1NDgsImF1ZCI6ImFwcHN0b3JlY29ubmVjdC12MSJ9.3GSXE32rGbhJtigWIP0sqKZU7bmQG-IOze8elz5iraDmXqmru0zGeQFh63nMYV_8GY0PzcE6h5ZpKaEEcnAxvw


```sh
curl  https://api.appstoreconnect.apple.com/v1/users --Header "Authorization: Bearer eyJraWQiOiJNMjRVSjVaNjJXIiwiYWxnIjoiRVMyNTYifQ.eyJpc3MiOiJkNDA0Y2FlMy1jZjViLTQ1ZjMtYmQ4My1iODU1OWJhMmI4ZDQiLCJleHAiOjE2MTEzMDM1NDgsImF1ZCI6ImFwcHN0b3JlY29ubmVjdC12MSJ9.3GSXE32rGbhJtigWIP0sqKZU7bmQG-IOze8elz5iraDmXqmru0zGeQFh63nMYV_8GY0PzcE6h5ZpKaEEcnAxvw"

curl  https://api.appstoreconnect.apple.com/v1/users --Header "Authorization: Bearer eyJraWQiOiJNMjRVSjVaNjJXIiwiYWxnIjoiRVMyNTYifQ.eyJpc3MiOiJkNDA0Y2FlMy1jZjViLTQ1ZjMtYmQ4My1iODU1OWJhMmI4ZDQiLCJleHAiOjE2MTEzMDM3MzAsImF1ZCI6ImFwcHN0b3JlY29ubmVjdC12MSJ9.xSkJCjMxADPX7V1JMdShAdo9XXCm3QRyOvBplqXjKYBOMc0SvsJOJkP49jAf8weTl_gXM5N4yZWuHUI4Er_5dw"

```

Authorization

Bearer eyJraWQiOiJNMjRVSjVaNjJXIiwiYWxnIjoiRVMyNTYifQ.eyJpc3MiOiJkNDA0Y2FlMy1jZjViLTQ1ZjMtYmQ4My1iODU1OWJhMmI4ZDQiLCJleHAiOjE2MTE1NDM1MzcsImF1ZCI6ImFwcHN0b3JlY29ubmVjdC12MSJ9.CnvwLwiIbkbUw-tX3DhIccBSO8Y9c2pQCNYg1hl0UDtl9nzSCAmrqoeafoAZMe-5pO4KLotR3_cWUpyvmDzFzQ


eyJhbGciOiJub25lIiwia2lkIjoiTTI0VUo1WjYyVyJ9.eyJleHAiOjEyMDAsImlzcyI6ImQ0MDRjYWUzLWNmNWItNDVmMy1iZDgzLWI4NTU5YmEyYjhkNCIsImF1ZCI6ImFwcHN0b3JlY29ubmVjdC12MSJ9.
