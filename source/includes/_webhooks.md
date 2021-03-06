# Webhooks

Quaderno's webhooks allow your application to receive information about events that happen on your documents as they occur.

## Data Format

```json
{
  "event_type":"invoice.created",
  "data":
  {
    "object":
    {
      "id":925,
      "contact_id":128,
      "tag_list":[],
      "number":"123346",
      "issue_date":"2016-02-12",
      "contact_name":"OrsonFarm",
      "currency":"EUR",
      "gross_amount_cents":826,
      "total_cents":1000,
      "amount_paid_cents":0,
      "po_number":null,
      "payment_details":null,
      "notes":null,
      "state":"outstanding",
      "subject":null,
      "street_line_1":null,
      "street_line_2":null,
      "city":null,
      "postal_code":null,
      "region":null,
      "country":"ES",
      "processor_id":null,
      "processor":null,
      "processor_fee_cents":null,
      "custom_metadata": {
        "my_custom_id": "123456"
      },
      "due_date":null,
      "permalink":"5191567e45d6aa419927ce7a8ba2ee870c9f0a45",
      "email":null,
      "gross_amount":"8.26",
      "total":"10.00",
      "amount_paid":"0.00",
      "items":
      [
        {
          "id":1056,
          "description":"Test item acces token",
          "quantity":"1.0",
          "unit_price":"8.26",
          "discount_rate":"0.0",
          "tax_1_amount_cents":174,
          "tax_1_name":"IVA",
          "tax_1_rate":21.0,
          "tax_1_country":null,
          "tax_2_amount_cents":0,
          "tax_2_name":null,
          "tax_2_rate":null,
          "tax_2_country":null,
          "subtotal_cents":"826.0",
          "total_amount_cents":1000,
          "discount_cents":"0.0",
          "taxes_included":true,
          "reference":null,
          "tax_1_amount":"1.74",
          "tax_2_amount":"0.00",
          "unit_price_cents":"826.00",
          "discount":"0.00",
          "subtotal":"8.26",
          "total_amount":"10.00"
        }
      ],
      "payments":[]
    }
  }
}

{
  "event_type":"contact.updated",
  "data":
  {
    "object":
    {
      "id":76,
      "kind":"person",
      "first_name":"Adella",
      "last_name":"Schowalter",
      "full_name":"Adella Schowalter",
      "contact_name":null,
      "street_line_1":null,
      "street_line_2":null,
      "postal_code":null,
      "city":null,
      "region":null,
      "country":"DE",
      "phone_1":null,
      "fax":null,
      "email":null,
      "web":null,
      "discount":null,
      "language":"EN",
      "tax_id":null,
      "currency":"USD",
      "notes":null,
      "processor_id":null,
      "processor":null
    }
  }
}

{
  "event_type":"payment.created",
  "data":
  {
    "object":
    {
      "id":15,
      "document_id":815,
      "date":"2015-09-22",
      "payment_method":"credit_card",
      "amount_cents":400,
      "amount":"4.00"
    }
  }
}

{
  "event_type":"checkout.succeeded",
  "data":{
    "object":{
      "transaction_details":{
        "session":43,
        "session_permalink":"https://demo.quadernoapp.com/checkout/session/8ccf3fdc42b85800188b113b81d3e4212ef094b3",
        "gateway":"stripe",
        "type":"charge",
        "description":"Unicorn",
        "customer":"cus_FXesSyaK3CG8Oz",
        "email":"john@doe.com",
        "transaction":"pi_1F2ZYtEjVHvINKlcq2as8H5V",
        "product_id":"prod_61ffa845b4a0b8",
        "tax_name":"VAT",
        "tax_rate":20.0,
        "extra_tax_name":null,
        "extra_tax_rate":null,
        "iat":1564647784,
        "amount_cents":1500,
        "amount":"15.00",
        "currency":"EUR"
      },
      "contact":{
        "id":547540,
        "kind":"company",
        "first_name":"John ",
        "last_name":"Doe",
        "full_name":"John  Doe",
        "contact_name":null,
        "street_line_1":"Fake Street 1",
        "postal_code":"SW15 5PU",
        "city":null,
        "region":null,
        "country":"GB",
        "email":"john@doe.com",
        "web":null,
        "language":"EN",
        "tax_id":null,
      }
    }
 }
}

{
  "event_type":"checkout.failed",
  "data":{
    "object":{
      "message":{
        "response_message":"Insufficient funds",
        "status_code":422
      },
      "transaction_details":{
        "gateway":"stripe",
        "type":"charge",
        "description":"Unicorn",
      },
      "contact":{
        "id":547540,
        "kind":"company",
        "first_name":"John ",
        "last_name":"Doe",
        "full_name":"John  Doe",
        "contact_name":null,
        "street_line_1":"Fake Street 1",
        "postal_code":"SW15 5PU",
        "city":null,
        "region":null,
        "country":"GB",
        "email":"john@doe.com",
        "web":null,
        "language":"EN",
        "tax_id":null,
      }
    }
  }
}

{
  "event_type":"checkout.abandoned",
  "data":{
    "object":{
      "transaction_details":{
        "description":"Unicorn",
        "plan":"awesome"
      },
      "contact":{
        "first_name":"John ",
        "last_name":"Doe",
        "city":null,
        "country":"GB",
        "email":"john@doe.com"
      }
    }
  }
}
```

Every webhook uses the same format for its data, regardless of event type. The webhooks take the form of a standard `POST`, with a hash using the following paramaters:

Parameter    | Description
-------------|-------------------------------------------------------------------------------------------------------
`event_type` | The event which triggered the webhook (`invoice.created`, `estimate.updated`, `contact.deleted`, etc).
`data`       | A simplified JSON representation of the object.

## Event Types

Event types are a combination of the object you want to be notified about and the object state.

Available events are:

- `invoice.created`, `invoice.updated`, `invoice.deleted`
- `receipt.created`, `receipt.updated`, `receipt.deleted`
- `credit.created`, `credit.updated`, `credit.deleted`
- `expense.created`, `expense.updated`, `expense.deleted`
- `estimate.created`, `estimate.updated`, `estimate.deleted`
- `payment.created`, `payment.deleted`
- `contact.created`, `contact.updated`, `contact.deleted`
- `checkout.succeeded`, `checkout.failed`, `checkout.abandoned`


For example, if you want to be notified whenever an invoice is created or deleted, you should subscribe to the events `invoice.created` and `invoice.deleted`.

## Endpoint errors

If your webhook-receiving endpoint does not respond with a `200` when Quaderno `POST`s the data, Quaderno will try again within 48 hours.

If you fail to respond a second time then your subscription to that webhook will be deleted.

<aside class="notice">
We recommend responding with a status code between 200 and 299 (inclusive) so that Quaderno does not assume there is an error in your endpoint.
</aside>

## Verifying webhook requests

Quaderno signs webhook requests so that you can (optionally) verify that requests were generated by Quaderno and not by a third-party impersonating us.

### Verify request signatures

Quaderno includes an additional HTTP header with webhook `POST` requests, `X-Quaderno-Signature`, which will contain the signature for the request.

To verify a request, generate a signature using the same key that Quaderno uses and compare that to the value of the `X-Quaderno-Signature` header.

### Get your webhook authentication key

When you create a webhook a key is automatically generated. If you're using `POST /webhooks.json` the key will be returned in the response.

To retrieve a webhook key via the Quaderno API, use `GET /webhooks.json` or `GET /webhooks/WEBHOOK_ID.json`.

### Generate a signature

```php?start_inline=1
/**
 * Generates a base64-encoded signature for a Quaderno webhook request.
 * @param string $webhook_key the webhook's authentication key
 * @param string $url the webhook url
 * @param array $body the request's POST parameters
 */
function generateSignature($webhook_key, $url, $body) {
    $signed_data = $url . $body;
    return base64_encode(hash_hmac('sha1', $signed_data, $webhook_key, true));
}
```

```ruby
require 'openssl'
require 'base64'

# Generates a base64-encoded signature for a Quaderno webhook request.
# @param string webhook_key the webhook's authentication key
# @param string url the webhook url
# @param array body the request's POST parameters
def generateSignature(webhook_key, url, body)
    signed_data = url + body
    Base64.encode64(OpenSSL::HMAC.digest('sha1', webhook_key, signed_data))
end
```

In your code that processes receieved webhooks:

1. Create a string with the webhook's URL, exactly as you entered it in Quaderno (including any query string, if applicable). Quaderno always signs webhook requests with the exact URL you provided when you configured the webhook. A difference as small as including or removing a trailing slash will prevent the signature from validating.
2. Append the `POST` body to the URL string, with no delimiter. The body should be a JSON-encoded representation of the data.
3. Hash the resulting string with `HMAC-SHA1`, using your webhook's authentication key to generate a binary signature.
4. Base64 encode the binary signature.
5. Compare the binary signature that you generated to the signature provided in the `X-Quaderno-Signature` HTTP header.

## Create a webhook

> POST /webhooks.json

```shell
# body.json
{
    "url": "http://anotherapp.com/notifications",
    "events_types": [
        "invoice.created",
        "estimate.updated",
        "invoice.deleted",
        "contact.created"
    ]
}

curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X POST \
     --data-binary @body.json \
     'https://ACCOUNT_NAME.quadernoapp.com/api/webhooks.json'
```

```ruby
params = {
    url: "http://anotherapp.com/notifications",
    events_types: [
        'invoice.created',
        'estimate.updated',
        'invoice.deleted',
        'contact.created'
    ]
}
Quaderno::Webhook.create(params) #=> Quaderno::Webhook
```

```php?start_inline=1
$webhook = new QuadernoWebhook(array(
                                 'url' => 'http://myapp.com/notifications',
                                 'events_types' => array('contact.created'));

$webhook->save(); // Returns true (success) or false (error)
```

`POST`ing to `/webhooks.json` will create a new webhook from the passed parameters.

Mandatory fields:

Field | Description
---|---
`url` | Indicates the destination URL of the webhook request.
`events_types` | An array of strings indicating which events you wish to subscribe to.

Webhook URLs should be set up to accept `HEAD` and `POST` requests. When you provide the URL where you want Quaderno to `POST` the data for events, we'll do a quick check that the URL exists by using a `HEAD` request (not `POST`).

If the URL doesn't exist or returns something other than a `200` HTTP response to the `HEAD` request, Quaderno won't be able to verify that the URL exists and is valid.

This will return `201 Created` with the current JSON representation of the webhook if the creation was a success.

## Retrieve: Get all webhooks

> GET /webhooks.json

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/ewbhooks.json'
```

```ruby
Quaderno::Webhook.all() #=> Array
```

```php?start_inline=1
$webhooks = QuadernoWebhook::find(); // Returns an array of QuadernoWebhook
```

```json
[
  {
    "id":2,
    "url":"https://myawesomeapp.com/notifications",
    "auth_key":"zXQgArTtQxAMaYppMrDoUQ",
    "events":["created","updated"],
    "last_sent_at":"2013-05-18T11:11:11Z",
    "last_error":null,
    "events_sent":null,
    "created_at":"2013-05-17T14:08:05Z",
    "updated_at":"2013-05-17T14:08:05Z"
  }
  {
    "id":3,
    "url":"http://anotherapp.com/notifications",
    "auth_key":"HXQgAgblQxAMaNppMrXoSW",
    "events:_types":["invoice.created","contact.deleted"],
    "last_sent_at":null,
    "last_error":null,
    "events_sent":null,
    "created_at":"2013-07-13T14:12:01Z",
    "updated_at":"2013-07-17T14:09:59Z"
  }
]
```

`GET`ting to `/webhooks.json` will return all your webhooks.

## Retrieve: Get a single webhook

> GET /webhooks/WEBHOOK_ID.json

```shell
curl -u YOUR_API_KEY:x \
     -X GET 'https://ACCOUNT_NAME.quadernoapp.com/api/webhooks/WEBHOOK_ID.json'
```

```ruby
Quaderno::Webhook.find(WEBHOOK_ID) #=> Quaderno::Webhook
```

```php?start_inline=1
$webhooks = QuadernoWebhook::find(WEBHOOK_ID); // Returns a QuadernoWebhook
```

```json

{
    "id":3,
    "url":"http://anotherapp.com/notifications",
    "auth_key":"HXQgAgblQxAMaNppMrXoSW",
    "events:_types":["invoice.created","contact.deleted"],
    "last_sent_at":null,
    "last_error":null,
    "events_sent":null,
    "created_at":"2013-07-13T14:12:01Z",
    "updated_at":"2013-07-17T14:09:59Z"
}
```

`GET`ting to `/webhooks/WEBHOOK_ID.json` will return a specific webhook.

## Update a webhook

> PUT /webhooks/WEBHOOK_ID.json

```shell
curl -u YOUR_API_KEY:x \
     -H 'Content-Type: application/json' \
     -X PUT \
     -d '{ "events_types": ["contact.updated","estimate.deleted"] }' \
     'https://ACCOUNT_NAME.quadernoapp.com/api/webhooks/WEBHOOK_ID.json'
```

```ruby
params = {
    events_types: [ 'contact.updated', 'estimate.deleted' ]
}
Quaderno::Webhook.update(WEBHOOK_ID, params) #=> Quaderno::Webhook
```

```php?start_inline=1
$webhook->url = "";
$webhook->save(); // Returns false - url is a required field
foreach($webhook->errors as $field => $errors) {
  print "{$field}: ";
  foreach ($errors as $e) print $e;
}

$webhook->url = 'http://anotherapp.com/quaderno/notifications';
$webhook->events_types = array('contact.created', 'contact.updated', 'contact.deleted');
$webhook->save();
```

`PUT`ting to `/webhooks/WEBHOOK_ID.json` will update a webhook with the passed parameters.

This will return `201 Created` with the current JSON representation of the webhook if the update was a success.

## Delete a webhook

> `DELETE /webhooks/WEBHOOK_ID.json`

```shell
curl -u YOUR_API_KEY:x \
     -X DELETE 'https://ACCOUNT_NAME.quadernoapp.com/api/webhooks/WEBHOOK_ID.json'
```

```ruby
Quaderno::Webhook.delete(WEBHOOK_ID) #=> Boolean
```

`DELETE`ing to `/webhooks/WEBHOOK_ID.json` will delete the specified webhook and return `204 No Content` if the update was successful.
