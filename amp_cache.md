# AMP Cache

## Refresh the AMP cache

The AMP cache can be refreshed by following the
[Update Amp Content](https://developers.google.com/amp/cache/update-cache) guide. The below steps are a simplified
version of the Google guide.

### 1
Generate a pair of RSA keys in the textual PEM format like this:
```
openssl genrsa 2048 > private-key.pem
openssl rsa -in private-key.pem -pubout >public-key.pem
```
### 2
Upload the public-key.pem file to the domain. It should go in /html/.well-known/amphtml/
### 3
Rename the public key to apikey.pub. Check it exists at https://example.com/.well-known/amphtml/apikey.pub
### 4
Use the private key to sign the update-cache request. Run this command:
```
echo -n >url.txt '/update-cache/c/s/example.com/article?amp_action=flush&amp_ts=1484941817' && cat url.txt | openssl dgst -sha256 -sign private-key.pem >signature.bin
```

(swap out example.com and 1484941817 for the domain and current timestamp.

### 5
Encode the base64 version of the signature.bin file you made in the last step. This command line will pop the base64
encoded signature on your clipboard
```
openssl base64 -in signature.bin | pbcopy
```

### 6
On your clipboard is now a base64 string. It's not URL safe though, so it needs some work. Swap out any +(plus)
characters for -(minus) and any /(forwardslash) characters for _(underscore). Remove and =(equals) characters. Pop the
string back on your clipboard.

### 7
Hit the following in your web browser.
 
 > https://example-com.cdn.ampproject.org/update-cache/c/s/example.com/article?amp_action=flush&amp_ts=1586357500&amp_url_signature=clipboard_paste

Swap out `example-com` and `example.com` for your domain, `1586357500` with the timestamp you used in step 4 and
`clipboard_paste` with the contents of your clipboard from step 5. You should see a blank white pags that says "OK".

### Notes
 - You must wait for the public key to be accessible in the AMP cache. You can check it at the following URL. Remember to
swap out www-gaborshoes-co-uk and www.gaborshoes.co.uk with your own domain. It should download your public key and
return no errors.

 > https://www-gaborshoes-co-uk.cdn.ampproject.org/r/s/www.gaborshoes.co.uk/.well-known/amphtml/apikey.pub

 - As annoying as it is, the timestamp you provide in the above steps must be within 60 seconds of the current time
 when you submit the request.
