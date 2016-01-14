# GoGoVan API documentation

### Getting a quote

Request:

<pre><code lang="bash">curl -X GET \
  -H 'GoGoVan-API-Key: b87d2da0-d333-4dbd-b984-b7d05d232fa3' \
  -H 'GoGoVan-User-Language: zh-TW' \
  -F 'order[name]=John' \
  -F 'order[phone_number]=61577364' \
  -F 'order[pickup_time]=2014-11-21T17:24:01Z' \
  -F 'order[vehicle]=motorcycle' \
  -F 'order[remark]=Please confirm the following 3 items: http://example.com/list' \
  -F 'order[locations]=[[22.312516,114.217874,"Seaview Center, 139 Hoi Bun Road, Hong Kong"],[22.282224,114.129262,"Kennedy Town, Hong Kong"]]' \
  'https://gogovan-staging-tw.herokuapp.com/api/v0/orders/price.json'
</code></pre>

`GoGoVan-User-Language` can be `en-US` `zh-TW` `zh-CN`

Response:

<pre><code lang="json">{
   "base" : 120,
   "total" : 230,
   "breakdown" : {
      "fee" : {
         "title" : "Fee",
         "value" : 120
      },
      "night_extra_charge" : {
         "value" : 100,
         "title" : "Night fee"
      },
   }
}
</code></pre>

### Requesting for a van

Request:

<pre><code lang="bash">curl -X POST \
  -H 'GoGoVan-API-Key: f817b69b-35cc-43d0-9eec-b5163cb87513' \
  -H 'GoGoVan-User-Language: zh-TW' \
  -F 'order[name]=John' \
  -F 'order[phone_number]=61577364' \
  -F 'order[pickup_time]=2014-12-12T17:24:01Z' \
  -F 'order[vehicle]=motorcycle' \
  -F 'order[remark]=Please confirm the following 3 items: http://example.com/list' \
  -F 'order[locations]=[[22.312516,114.217874,"Seaview Center, 139 Hoi Bun Road, Hong Kong"],[22.282224,114.129262,"Kennedy Town, Hong Kong"]]' \
  'https://gogovan-staging-tw.herokuapp.com/api/v0/orders.json'
</code></pre>

Response:

<pre><code lang="json">{ "id": 729 }
</code></pre>

### Canceling a request

<pre><code lang="bash">curl -v -X POST \
  -H 'GoGoVan-API-Key: f817b69b-35cc-43d0-9eec-b5163cb87513' \
  -H 'GoGoVan-User-Language: zh-TW' \
  'https://gogovan-staging-tw.herokuapp.com/api/v0/orders/729/cancel.json'
</code></pre>

The response will be just HTTP 200 OK with no response body.

### Checking status

Request:

<pre><code lang="bash">curl -v -X GET \
  -H 'GoGoVan-API-Key: f817b69b-35cc-43d0-9eec-b5163cb87513' \
  -H 'GoGoVan-User-Language: zh-TW' \
  'https://gogovan-staging-tw.herokuapp.com/api/v0/orders/729.json'
</code></pre>

Response:

<pre><code lang="json">{
   "name" : "John",
   "price" : 220,
   "id" : 729,
   "phone_number" : "61577364",
   "status" : "pending"
}</code></pre>

### Getting status notification

An HTTP JSON POST request will be made to a designated URL when the status of an Order changed. It will contain the response in the format of _checking status_.

It will also contain a field named `event`, with the following possible values:

<pre>
assigned - A driver has been assigned to the order
completed - The order has been marked "completed" by the driver
canelled - The order has been marked "cancelled" by the driver
</pre>

