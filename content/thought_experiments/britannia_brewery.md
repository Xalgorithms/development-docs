---
layout: default
title: The Britannia Brewery
parent: Thought Experiments
nav_order: 1
---

# Context

In the spring of 2020, we attempted to finish the [latest
version](https://github.com/orgs/Xalgorithms/projects/8) of the the Xalgo
language. We brought in some new collaborators that brought a different
perspective on how to do _table-based programming_. This lead us to start to
reconsider the capabilities of the language and even the scope of the
language. It was proposed that Xalgo could be _far simpler_ than it is. It might
even be possible to express what we needed to express in a set of
_spreadsheet-like tables_, rather than in a textual programming language. This
experiment is designed to test the results of those conversations.

# The Story

The Britannia Brewing Company is an local Ottawa brewing company with an eye to
the future. They've implemented an end-to-end brewery management information
system. It's been a great success in automating the production and online,
direct-to-customer sales and delivery. They've decided to offer their specialize
e-commerce software to other breweries.

Britannia Brewing is a forward-looking company, so they've decided to discard
much of their site-specific logic and replace it with rules managed and executed
within Interlibr. It's their hope that - if they provide the initial
implementation - other breweries will modify or extend the rules that they will
have written for Interlibr. As a proof-of-concept, Britannia will implement
three classes of rules that they seen over and over while implementing a
solution for their company.

1. _Delivery_: These focus on rules that offer one of more delivery options
   to customers based on location and spot discounts for order volume.
   
1. _Promotional_: Over their years in operation, Brittania Brewing has often
   offered discounts or free extras on orders that meet certain criteria, for a
   limited period.
   
1. _Loyalty_: Repeat customers have been the life-blood of the
   brewery. Britannia often offers promotional items to regular customers.
   
# The Experiment

Britannia wants to understand how they would have to integrate with Interlibr
both as a _rule taker_ and a _rule maker_. They have provided the information
that they have distilled from their site-specific implementation of these
classes of rules. In the past, they have found the provided information to be
all that is needed to make context-independent abstractions of rules for their
platform. They need our help to understand if this data will be sufficient for
Interlibr. As a _rule maker_, they would like to learn the capabilities and
limitations of Interlibr by understanding the implementation of one rule from
each class.

_Britannia anticipates submitting **each** order individually to
Interlibr. Please instruct if multiple orders should be submitted as a batch._

# The Rules

## Delivery Options

Britannia offers several delivery options for customers, including local
delivery. These are the criteria for their delivery options:

- _Local Delivery_: If the order is to be delivered in Ottawa, then local
  delivery is available at a flat rate of 15$.
- _Free local delivery_: If a buyer spends 60$ before tax or orders 24 or more
  cans, then local delivery is free.
- _Canada Post_: Orders can be delivered anywhere in Ontario using Canada
  Post. The rate is 6$ per 12 cans.
  
The website for the brewery is available worldwide, without geofencing. All
buyers are welcome, but regulations require that only Ontario, Canada deliveries
are permitted.

## Anniversary Party

This is an example of a promotional rule that Britannia ran on their fifth
anniversary.

- All orders receive 2 brewery branded coasters.
- Orders over 20$ get a pint glass.
- Orders over 50$ get an anniversary t-shirt.
- Orders over 100$ get 2 pint glasses and the t-shirt.

This promotion was active from 2020 June 12 - 19.

## To Our Loyal Tasters

This was a loyalty promotion that featured some new styles of beer that the
brewery was offering. It was offered from 2020 June - July.

- To promote a new West Coast IPA, if the buyer had ordered beers that had a sum
  of greater then 300 IBU in the promotional period, then they got a "Bitter is
  Best" t-shirt.
- To promote their new British Porter, if the buyer had ordered more than 2
  dozen cans of their english style range in the last 6 months, they get a
  promotional pint glass with a stylized "Dominion of Canada" brewery logo
  designed by a local artist. There must be at least 4 cans of the new porter in
  the qualifying order.
  
In each of these loyalty offerings, the current order was considered as part of
the total tally.

# The Data

The platform has two mechanisms for providing data to external systems. As part
of the experiment, the development team needs to know _how_ to send this data to
Interlibr.

## Orders

Britannia Brewing's platform is capable of exporting _individual orders_ from
customers to SOML (Standard Order Markup Language). This is a W3C-approved
format derived from UBL that captures a orders from an e-commerce site. It can
be sent in XML or JSON format. Britannia, for performance reasons, _only
supports JSON_.

This is an example of an order exported into SOML/json:

```
{
  "id" : "38936c5e",
  "customer" : {
    "id" : "72cb5602",
    "address" : {
      "street" : "Main St",
      "number" : "12",
      "city" : "Ottawa",
      "subentity" : {
        "name" : "Ontario",
        "code" : {
          "value" : "CA-ON",
          "list_id" : "ISO 3116-2",
          "list_name" : "Country Subentity",
          "version_id" : "20010914"
        }
      }
    }
  },
  "items" : [
    {
      // 6 cans of 'The MSB' priced at 5.76CAD per 1 can
      "id" : {
        "value" : "1",
        "list_id" : "britannia-stock-ids"
      },
      "description" : "The MSB (6)",
      "quantity" : {
        "value" : 6,
        "unit" : "can"
      },
      "pricing" : {
        "price" : {
          "value" : "5.76",
          "currency_code" : "CAD"
        },
        "quantity" : {
          "value" : 1,
          "unit" : "can"
        }
      }
    },
    {
      // 12 cans of 'Eternally Hoptimistic' priced at 4.87CAD per 1 can
      "id" : {
        "value" : "3",
        "list_id" : "britannia-stock-ids"
      },
      "description" : "Eternally Hoptimistic (12)",
      "quantity" : {
        "value" : 12,
        "unit" : "can"
      },
      "pricing" : {
        "price" : {
          "value" : "4.87",
          "currency_code" : "CAD"
        },
        "quantity" : {
          "value" : 1,
          "unit" : "can"
        }
      }
    }
  ]
}
```

## Other data

The Britannia platform has been integrated with other software in the past, so
the development team has implemented a table-based export of _order relevant
data_. For this experiment, they determined that their `customer_history` and
`stock` tables are the most relevant. If other data is needed, let the team know
and they'll make other tables available.

The `customer_history` table enumerates past orders for the user associated with
an order:

| id       | name              | stock_id | stock_name            | order_id | quantity | order_date                |
|----------|-------------------|----------|-----------------------|----------|----------|---------------------------|
| 27eb11cd | Laverne DeFazio   | 1        | Dirty Blonde          | d4af0131 | 12       | 2020-06-13T00:30:24-04:00 |
| 675b70ec | Shirley Feeney    | 3        | Eternally Hoptimistic | 3b9e255d | 24       | 2020-06-13T00:30:24-04:00 |
| 6bc91ad2 | Leonard Kosnowski | 2        | Amber Rose            | 0e3b047d | 48       | 2020-06-13T00:30:24-04:00 |
| 72cb5602 | Andrew Squiggman  | 5        | The MSB               | 38936c5e | 6        | 2020-06-13T00:30:24-04:00 |
| 72cb5602 | Andrew Squiggman  | 3        | Eternally Hoptimistic | 38936c5e | 12       | 2020-06-13T00:30:24-04:00 |
| 72cb5602 | Andrew Squiggman  | 7        | Shed No Tears         | 95475c5c | 12       | 2020-06-13T00:30:24-04:00 |

The `order_date` column is an ISO8601 date. The `id` and `order_id` are unique,
random hexes generated at first use.

The `stock` table always includes the list of past and current beers that the
brewery offers:

| id | name                  | family   | style  | IBU  |
|----|-----------------------|----------|--------|------|
| 1  | Dirty Blonde          | american | blonde | 20   |
| 2  | Amber Rose            | american | amber  | 30   |
| 3  | Eternally Hoptimistic | american | pale   | 38.5 |
| 4  | Walk on the Mild Side | british  | mild   | 20   |
| 5  | The MSB               | british  | brown  | 45   |
| 6  | After Midnight        | british  | stout  | 20   |
| 7  | Shed No Tears         | british  | stout  | 15   |
| 8  | The Darkess           | british  | porter | 25   |
