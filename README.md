# ngrok Traffic Policy Examples

Below are a few examples that I hope others will find useful when using the new Ngrok Traffic Policy Module.

### Static HTML, great if you like Pizza !
Sample Statis HTML Page

```
enabled: true
inbound:
  - expressions: []
    name: ""
    actions:
      - type: custom-response
        config:
          status_code: 200
          content: >
            <!DOCTYPE html>

            <html lang="en">
              <head>
                <meta charset="UTF-8">
                <meta name="viewport" content="width=device-width, initial-scale=1.0">
                <style>
                  html {
                    height: 100%;
                  }
                  body {
                    background: url(https://i.imgur.com/Fhp5EwR.png) repeat;
                    background-size: 10%;
                    display: flex;
                    align-items: center;
                    justify-content: center;
                    color: white;
                    text-shadow: 0 10px 5px black;
                    height: 100%;
                  }

                  .rainbow {
                    text-align: center;
                    font-size: 5rem;
                    font-family: monospace;
                    letter-spacing: 5px;
                    animation: colorRotate .5s linear 0s infinite;
                  }

                  @keyframes colorRotate {
                    from {
                      color: #6666ff;
                    }
                    10% {
                      color: #0099ff;
                    }
                    50% {
                      color: #00ff00;
                    }
                    75% {
                      color: #ff3399;
                    }
                    100% {
                      color: #6666ff;
                    }
                  }
                </style>
              </head>
              <body>
                <h1 class="rainbow">THATS A NICE LARGE PIZZA</h1>
              </body>
            </html>
outbound: []
```

### Rewrite a URL !
This is **SUPER** useful with ngrok, sometimes it's hard to change URL's this can help you with that ! Until now we could not do this with ngrok 
```
inbound:
  - expressions:
      - hasReqHeader('Host') != true
    name: Add preweb to request Path
    actions:
      - type: url-rewrite
        config:
          from: ^[^#]*?://.*?(/.*)$
          to: $host/prweb/preservlet$1
outbound:  []
```
<br/>

### Look for a Specific Header
It might be redundant, but I added an Expression to make sure the `Baz` Header was set.
```
inbound:
  - expressions:
      - req.Headers['Baz'].exists(key, key != 'Fizz')
    name: Header check
    actions:
      - type: deny
  - expressions:
      - hasReqHeader('Baz') != true
    name: Header check2
    actions:
      - type: deny
outbound: []
```
<br/>

### Look for a Query Param
Just Make sure a specific Query Param is passed, might be good to block invalid requests heading to API Gateways
```
inbound:
  - expressions:
      - "!('secret' in getQueryParam('token'))"
    name: Secret not Found in Token Query Parameter
    actions:
      - type: deny
outbound: []
```
<br/>

### Check for Referrer Header
Not all browsers support this, so use at your own risks. I had a Customer who wanted to use this.
```
  - expressions:
      - hasReqHeader('referrer') != true
    name: Header check2
    actions:
      - type: deny
```
<br/>


