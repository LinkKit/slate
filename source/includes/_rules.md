# Rules

> Example ruleset:

```json
[
  /*
    Define a rule that makes the link respond with a string
    when a X-Action: respond header is included in the request
  */
  {
    "id": "unique-id-1",
    "conditions": {
      "any": [
        {
          "fact": {
            "header": "X-Action"
          },
          "operator": "equals",
          "value": "respond"
        }
      ]
    },
    "action": {
      "type": "respond",
      "params": {
        "body": "Hello, Mr. Anderson"
      }
    }
  },
  /*
    Define a rule that makes the link redirect to an alternative url
    if the X-Action: redirect header is included in a POST request
  */
  {
    "id": "unique-id-2",
    "conditions": {
      "all": [
        {
          "fact": {
            "header": "X-Action"
          },
          "operator": "equals",
          "value": "redirect"
        },
        {
          "fact": "method",
          "operator": "equals",
          "value": "POST"
        }
      ]
    },
    "action": {
      "type": "redirect",
      "params": {
        "url": "https://imgur.com/X17puIB"
      }
    }
  }
  /* If no rules are matches the default the link will redirect to the default url */
]
```

Rules are a list of independent sets of conditions. When a rules conditions evaluate to true the rules defined alternative action is executed rather than the default URL redirect.

Property | Description
-------- | -----------
id | Opaque id, should be unique to list of rules for this link
conditions | A conditions object combining any/all matches against "facts"
action | Object defining the action to be executed if rule conditions are met
action.type | "redirect","respond"
action.params.url | url to redirect to when action.type is redirect
action.params.body | body to respond with when action.type is respond

Conditions must have a top level `any` or `all` item, any amount of `any`/`all` items can be nested.

Condition | Description
--------- | -----------
any | any of child rules may match for condition to be true
all | all of child rules must match for condition to be true
fact | object containing header key or another fact like "method"
fact.header | define a header to match against
operator | how to compare fact against value, currently supports: "equals"
value | string value to compare facts against
