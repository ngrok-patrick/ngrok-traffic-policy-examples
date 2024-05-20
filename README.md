# ngrok Traffic Policy Examples

Below are a few examples that I hope others will find useful when using the new Ngrok Traffic Policy Module.

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
**Not All** browsers support this, so use at your own risks. I had a Customer who wanted to use this.
```
  - expressions:
      - hasReqHeader('referrer') != true
    name: Header check2
    actions:
      - type: deny
```
<br/>
### 


