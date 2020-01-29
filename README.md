# SendPostal API documentation

#### Send postal letters online with the SendPostal API or with the web app [sendpostal.io](https://app.sendpostal.io)

# Guide

## Get your API key

- Create an account on [app.sendpostal.io](https://app.sendpostal.io/account)
- Click `Generate API key`
- Replace `<YOUR API KEY>` in the documentation below with your API key

## Send a letter

You can send a letter directly or get a quote and preview before sending

### Send in two steps

First upload the file to get a quote and then send it.

#### Step 1: Upload the file

Send a first POST request to the `upload` api endpoint with the parameter `send` set to `false` for JSON and `0` for Multipart Form to prevent the letter from being sent directly.

**Example Multipart Form Request**

```shell
curl --request POST \
  --url https://api.sendpostal.io/upload \
  --header 'authorization: Bearer <YOUR API KEY>' \
  --header 'content-type: multipart/form-data' \
  --form file=/tmp/pdfToSend.pdf \
  --form send=0 \
  --form color=0 \
  --form senderName=Elon Musk \
  --form senderOrganization=SpaceX \
  --form senderAddress=12520 Wilkie Ave, Gardena, CA 90249 \
  --form recipientName=Elon Musk \
  --form recipientOrganization=SpaceX \
  --form recipientAddress=12520 Wilkie Ave, Gardena, CA 90249 \
  --form recipientCountryCode=US
```

**Example JSON Request**

```shell
curl --request POST \
  --url https://api.sendpostal.io/upload \
  --header 'authorization: Bearer <YOUR API KEY>' \
  --header 'content-type: application/json' \
  --data '{
	"document": {
		"type": "documentUrl",
		"documentUrl": "https://api.xero.com/file/234234"
	},
	"send": false,
	"color": false,
	"sender": {
		"name": "Elon Musk",
		"organization": "SpaceX",
		"address": "12520 Wilkie Ave, Gardena, CA 90249",
		"countryCode": "US"
	},
	"recipient": {
		"name": "Elon Musk",
		"organization": "SpaceX",
		"address": "12520 Wilkie Ave, Gardena, CA 90249",
		"countryCode": "US"
	}
}'
```

The response from the API will contain the cost of sending the letter, the remaining credit, a link to the PDF being sent and the urls of the thumbnails of the PDF for preview.

The response also contains the `sendUrl` that can be use to send the letter later.

**Example Response**

```js
{
    "letterId": "34g7h8-4s68dfs-997j55f77d5",
    "status": "readyToSend",
    "color": false,
    "sender": {
    	"name": "Elon Musk",
    	"organization": "SpaceX",
    	"address": "12520 Wilkie Ave, Gardena, CA 90249",
    	"countryCode": "US"
    },
    "recipient": {
    	"name": "Elon Musk",
    	"organization": "SpaceX",
    	"address": "12520 Wilkie Ave, Gardena, CA 90249",
    	"countryCode": "US"
    },
    "price": 350, // $3.50
    "currentCredit": 12450, // $124.50
    "pdf": "https://api.sendpostal.io/pdf/234234",
    "sendUrl": "https://api.sendpostal.io/send/34g7h8-4s68dfs-997j55f77d5",
    "thumbnails": [
    	"https://api.sendpostal.io/thumbnail/234234/1",
    	"https://api.sendpostal.io/thumbnail/234234/2",
    	"https://api.sendpostal.io/thumbnail/234234/3",
    	"https://api.sendpostal.io/thumbnail/234234/4"
    ]
}
```

#### Step 2: Send the file

Send a POST request to the url contained in the `sendUrl` field with your token in the `authorization` header as follow

**Example send letter request**

```shell
curl --request POST \
  --url https://api.sendpostal.io/send/34g7h8-4s68dfs-997j55f77d5 \
  --header 'authorization: Bearer <YOUR API KEY>'
```

If successfull, this second request will receive this response

```js
{
    "letterId": "34g7h8-4s68dfs-997j55f77d5",
    "status": "sent",
    "color": false,
    "sender": {
    	"name": "Elon Musk",
    	"organization": "SpaceX",
    	"address": "12520 Wilkie Ave, Gardena, CA 90249",
    	"countryCode": "US"
    },
    "recipient": {
    	"name": "Elon Musk",
    	"organization": "SpaceX",
    	"address": "12520 Wilkie Ave, Gardena, CA 90249",
    	"countryCode": "US"
    },
    "price": 350, // $3.50
}
```

### Send the letter directly

To send the letter directly without getting a quote first, send a `POST` request to the `https://api.sendpostal.io/upload` api endpoint with the `send` field set to `true` in JSON and `1` in Multipart Form.

This will upload and send the letter directly.

**Example JSON Request**

```shell
curl --request POST \
  --url https://api.sendpostal.io/upload \
  --header 'authorization: Bearer <YOUR API KEY>' \
  --header 'content-type: application/json' \
  --data '{
	"document": {
		"type": "documentUrl",
		"documentUrl": "https://api.xero.com/file/234234"
	},
	"send": true,
	"color": false,
	"sender": {
		"name": "Elon Musk",
		"organization": "SpaceX",
		"address": "12520 Wilkie Ave, Gardena, CA 90249",
		"countryCode": "US"
	},
	"recipient": {
		"name": "Elon Musk",
		"organization": "SpaceX",
		"address": "12520 Wilkie Ave, Gardena, CA 90249",
		"countryCode": "US"
	}
}'
```

**Example Multipart Form Request**

```shell
curl --request POST \
  --url https://api.sendpostal.io/upload \
  --header 'authorization: Bearer <YOUR API KEY>' \
  --header 'content-type: multipart/form-data' \
  --form file=/tmp/pdfToSend.pdf \
  --form send=1 \
  --form color=0 \
  --form senderName=Elon Musk \
  --form senderOrganization=SpaceX \
  --form senderAddress=12520 Wilkie Ave, Gardena, CA 90249 \
  --form recipientName=Elon Musk \
  --form recipientOrganization=SpaceX \
  --form recipientAddress=12520 Wilkie Ave, Gardena, CA 90249 \
  --form recipientCountryCode=US
}
```

**Example Response**

```js
{
    "letterId": "34g7h8-4s68dfs-997j55f77d5",
    "status": "sent",
    "color": false,
    "sender": {
    	"name": "Elon Musk",
    	"organization": "SpaceX",
    	"address": "12520 Wilkie Ave, Gardena, CA 90249",
    	"countryCode": "US"
    },
    "recipient": {
    	"name": "Elon Musk",
    	"organization": "SpaceX",
    	"address": "12520 Wilkie Ave, Gardena, CA 90249",
    	"countryCode": "US"
    },
    "price": 350, // $3.50
    "currentCredit": 12450, // $124.50
    "pdf": "https://api.sendpostal.io/pdf/234234",
    "sendUrl": "https://api.sendpostal.io/send/34g7h8-4s68dfs-997j55f77d5",
    "thumbnails": [
    	"https://api.sendpostal.io/thumbnail/234234/1",
    	"https://api.sendpostal.io/thumbnail/234234/2",
    	"https://api.sendpostal.io/thumbnail/234234/3",
    	"https://api.sendpostal.io/thumbnail/234234/4"
    ]
}
```

# Reference

## Upload Letter Request

Endpoint: `api.sendpostal.io/upload`

Method: `POST`

### Multipart form request
	
| Request Header |           Value        | Required|
|----------------|:----------------------:|:--------:| 
| `Content-Type` |  `multipart/form-data` | true |
| `Authorization` |  `Bearer <YOUR API KEY>` | true | 
	
	

| Request Body Field | Description | Type | Default value | Required | Example |
|--------------------|:-----------:|:----:|:-------:|:--------:|:-------:| 
| `file` |  Path of the PDF file to send | String | none | true | `/tmp/fileToSend.pdf`
| `send` |  Send the letter directly | Boolean | `false` | true | `true`
| `color` |  Print PDF in color | Number (`1` for color `0` for black and white) | `0` (no color) | true | 1
| `senderName` |  Name of the sender | String | none | false | `Elon Musk`
| `senderOrganization` |  Name of the sender's organization | String | none | false | `SpaceX`
| `senderAddress` |  Address of the sender | String | none | false | `12520 Wilkie Ave, Gardena, CA 90249`
| `senderCountryCode` |  Country of the sender | See `Country Codes` below | none | false | `US`
| `recipientName` |  Name of the recipient | String | none | true | `Elon Musk`
| `recipientOrganization` |  Name of the recipient's organization | String | none | false | `SpaceX`
| `recipientAddress` |  Address of the recipient | String | none | true | `12520 Wilkie Ave, Gardena, CA 90249`
| `recipientCountryCode` |  Country code of the recipient | see `Country Codes` | none | true | `US`


### JSON request


| Request Header |        Value        | Required | 
|----------------|:-------------------:|:--------:| 
| `Content-Type` |  `application/json` | true | 
| `Authorization` |  `Bearer <YOUR API TOKEN>` | true | 


| Request Body Field | Description | Type | Default value | Required | Example |
|--------------------|:-----------:|:----:|:-------:|:--------:|:-------:| 
| `document` |  Path of the PDF file to send | `Document` see bellow | none | true | `/tmp/fileToSend.pdf`
| `send` |  Send the letter directly | Boolean | `false` | true | `true`
| `color` |  Print PDF in color | Number (`1` for color `0` for black and white) | `0` (no color) | true | 1
| `sender` |  sender as a JSON object | `Contact` see below | {} | false | ``` { "name": "Elon Musk", "organization": "SpaceX", "address": "12520 Wilkie Ave, Gardena, CA 90249", "countryCode": "US" }```
| `recipient` |  recipient as a JSON object | `Contact` see below | {} | true |``` { "name": "Elon Musk", "organization": "SpaceX", "address": "12520 Wilkie Ave, Gardena, CA 90249", "countryCode": "US" }```


## Send Letter Request

Endpoint: `api.sendpostal.io/send/<ID OF THE LETTER TO SEND>`

Method: `POST`

| Request Header |        Value        | Required | 
|----------------|:-------------------:|:--------:| 
| `Authorization` |  `Bearer <YOUR API KEY>` | true | 


## Upload or Send Letter Response

| Response Body Field | Description | Type |
|--------------------|:-----------:|:----:|
| `letterId ` |  Id of the related letter | String |
| `status` |  Status of the letter | String can be `sent`, `readyToSend` or `error` |
| `color` |  Whether the letter is set to be printed in color | Number (`1` for color `0` for black and white)
| `sender` |  sender as a JSON object | `Contact` see below 
| `recipient` |  recipient as a JSON object | `Contact` see below
| `price ` |  The calculated shipping price | A positive integer in the smallest currency unit (e.g., 100 cents to charge $1.00 or 100 to charge ¥100, a zero-decimal currency)
| `currentCredit ` |  Current credit of the user | A positive integer in the same format as the price
| `sendUrl ` | The url to post a request to in order to send the letter if it has not been sent yet | String
| `pdf` |  The http link to download the PDF of the letter to be sent | String
| `thumbnails ` |  The list of the urls of the thumbnails of the letter for preview | Array of String

## Get quote Request

## Get quote Response

## Types

### Contact

A contact is a JSON object containing the information of the sender or the recipient of a letter.

| Field | Type | Required |
|------|:-------:|:-----------:|
| `name` | String | only for recipient |
| `organization ` | String | false |
| `address ` | String | only for recipient |
| `countryCode` | See `Country Code` below | only for recipient |

Example

```js
{
	"name": "Elon Musk",
	"organization": "SpaceX",
	"address": "12520 Wilkie Ave, Gardena, CA 90249",
	"countryCode": "US"
}
```


### Document

A document is a JSON object that contains a `type` to determine the source of the document and other fields depending on the type detailed bellow.

The `type` of the document is a **required** String and can be

- `documentUrl`
- `googleDocId`


#### Send a document from url (`documentUrl`)

| Field | Type | Required |
|------|:-------:|:-----------:|
| `type` | `documentUrl ` | true |
| `fileUrl ` | String | true |

Example

```json
{
	"type": "documentUrl",
	"fileUrl": "https://api.xero.com/file/234234"
}
```

#### Send a document from Google Doc (`googleDoc`)

| Field | Type | Required |
|------|:-------:|:-----------:|
| `type` | `googleDoc ` | true |
| `googleDocId ` | String | true |
| `googleDocToken ` | String | true |

Example

```json
{
	"type": "googleDoc",
	"googleDocId": "https://docs.google.com/document/d/3dqdMsqWX_orBfG_6UQsdfwm-YYQqsdGONmwg4",
	"googleDocToken": "sFRSdsqWXYYQqssFRSdsqWXYYQqsdG33mwq44dGON633mwq4sFRSdsqWXYYQqsdGON633mwq1204"
}
```

### Country Code


Get the JSON file containing the list of the supported country codes [here](./countries.json)


## Errors

| Code | Message | Description |
|------|:-------:|:-----------:|
| `500` |  `No document available` | You need to specify a document |
