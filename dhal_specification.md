---
layout: default
---
# DHAL - Data-template Hypertext Application Language

## A lean hypermedia type based on HAL with extensions for dtime.com


* __Author:__ Mike Kelly ([mike@stateless.co][1])
* __Dhal Edits:__ David Haslem ([david@dtime.com][1])
* __Dates:__ 2012-05-15 (Forked) 2012-05-15 (Last Updated)
* __Status:__ Draft

### Discussion Group

This is an extension of the HAL media type. The hal mailing list is available here:
[HAL-discuss mailing list][2].

## Notes

DHAL is a superset of HAL. This document is a fork of the original HAL specification
available here: <http://stateless.co/hal_specification.html>

As it is intended to be a superset, most of this document will describe HAL.
DHAL-specific sections will be marked with an asterisk (\*), and are listed here for
quick reference:

* the `_template` attribute is now part of the formal definition
* A link may have the `href-template` instead of `href` attribute set. If it is
  set, it will be a URI template and should conform with the rules in rfc 6570:
  <http://tools.ietf.org/html/rfc6570>. To conform with the current draft of
  HAL, the href-template may also be stored in the href - if both are present,
  the href-template is to be preferred.

## General Description

HAL is a simple way of linking with JSON or XML.

It provides a set of conventions for expressing hyperlinks to, and embeddedness of, related resources - the rest of a HAL document is just plain old JSON or XML.

HAL is a bit like HTML for machines, in that it is designed to drive many different types of application. The difference is that HTML is intended for presenting a graphical hypertext interface to a 'human actor', whereas HAL is intended for presenting a machine hypertext interface to 'automated actors'.

This document contains a formalised specification of HAL. For a friendlier, more pracitcal introduction to HAL you can read this article: [JSON Linking with HAL][3]

HAL has two main components: Resources and Links.
* Resources can have their own state, links, and other embedded resources.
* Links have a link relation (rel) that signals how to interpret the target resource.

Below is an image illustrating HAL's information model:

![The HAL Information model][4]

HAL is two media types (application/hal+json & application/hal+xml) with which applications are meant to be developed and exposed as sets of traversable link relations.

Instead of using linkless JSON/XML, or spending time developing a custom
media type, you can just use HAL and focus on creating link relations to
drive your application.

HAL encourages the use of link relations to:

*   Identify links and embedded resources within the representation
*   Infer the expected structure and meaning of target resources
*   Signalling what requests and representations can be submitted to target resources

## Examples

Here is how you could represent a collection of orders with the JSON variant of HAL:

{% highlight javascript %}
{
  "_links": {
    "self": { "href": "/orders" },
    "next": { "href": "/orders?page=2" },
    "search": { "href": "/orders?id={order_id}" }
  },
  "_embedded": {
    "order": [
      {
        "_links": {
          "self": { "href": "/orders/123" },
          "customer": { "href": "/customer/bob", "title": "Bob Jones <bob@jones.com>" }
        },
        "total": 30.00,
        "currency": "USD",
        "status": "shipped",
        "placed": "2011-01-16",
        "_embedded": {
          "basket": {
            "_links": {
              "self": { "href": "/orders/123/basket" }
            },
            "items": [
              {
                "sku": "ABC123",
                "quantity": 2,
                "price": 9.50
              },{
                "sku": "GFZ111",
                "quantity": 1,
                "price": 11
              }
            ]
          }
        }
      },{
        "_links": {
          "self": { "href": "/orders/124" },
          "customer": { "href": "/customer/jen", "title": "Jen Harris <jen@internet.com>" }
        },
        "total": 20.00,
        "currency": "USD",
        "status": "processing",
        "placed": "2011-01-16",
        "_embedded": {
          "basket": {
            "_links": {
              "self": { "href": "/orders/124/basket" }
            },
            "items": [
              {
                "sku": "KLM222",
                "quantity": 1,
                "price": 9.00
              },{
                "sku": "HHI50",
                "quantity": 1,
                "price": 11.00
              }
            ]
          }
        }
      }
    ]
  }
}
{% endhighlight %}

Here is the same example using the XML variant of HAL:

{% highlight xml %}
<resource href="/orders">
  <link rel="next" href="/orders?page=2" />
  <link rel="search" href="/orders?id={order_id}" />
  <resource rel="order" href="/orders/123">
    <link rel="customer" href="/customer/bob" title="Bob Jones <bob@jones.com>" />
    <resource rel="basket" href="/orders/123/basket">
      <item>
        <sku>ABC123</sku>
        <quantity>2</quantity>
        <price>9.50</price>
      </item>
      <item>
        <sku>GFZ111</sku>
        <quantity>1</quantity>
        <price>11.00</price>
      </item>
    </resource>
    <total>30.00</total>
    <currency>USD</currency>
    <status>shipped</status>
    <placed>2011-01-16</placed>
  </resource>
  <resource rel="order" href="/orders/124">
    <link rel="customer" href="/customer/jen" title="Jen Harris <jen@internet.com>" />
    <resource rel="basket" href="/orders/124/basket">
      <item>
        <sku>KLM222</sku>
        <quantity>1</quantity>
        <price>9.00</price>
      </item>
      <item>
        <sku>HHI50</sku>
        <quantity>1</quantity>
        <price>11.00</price>
      </item>
    </resource>
    <total>20.00</total>
    <currency>USD</currency>
    <status>processing</status>
    <placed>2011-01-16</placed>
  </resource>
</resource>
{% endhighlight %}

## Code
* [(Ruby) JSON::HAL][16]
* [(PHP) Hal Library][12]
* [(PHP) Nocarrier\Hal][15]
* [(C#) Hal.Net][13]
* [(C#) WCF Media Type Formatter][14]
* [(Java) HalBuilder][11]

## Compliance

An implementation is not compliant if it fails to satisfy one or more of the MUST or REQUIRED level requirements. An implementation that satisfies all the MUST or REQUIRED level and all the SHOULD level requirements is said to be "unconditionally compliant"; one that satisfies all the MUST level requirements but not all the SHOULD level requirements is said to be "conditionally compliant."

> The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119][5].

## Media Type Identifiers
*   __application/hal+json__

    The JSON based variant of HAL

*   __application/hal+xml__

    The XML based variant of HAL

## Components

HAL provides hypertext capabilities via two elements:

1.  Resources

    For expressing the embedded nature of a given part of the representation.

2.  Links

    For expressing 'outbound' hyperlinks to other, related resources.

### Shared Attributes

The **Resource** and **Link** elements share the following attributes:

*   __href__

    REQUIRED

    For indicating the target URI.

    **href** corresponds with the '[Target IRI][6]' as defined in [Web Linking (RFC 5988)][7]

*   __rel__

    REQUIRED

    For identifying how the target URI relates to the 'Subject **Resource**'. The Subject **Resource** is the closest parent **Resource** element.

    This attribute is not a requirement for the root element of a HAL representation, as it has an implicit default value of 'self'

    **rel** corresponds with the '[relation parameter][8]' as defined in [Web Linking (RFC 5988)][7]

    **rel** attribute SHOULD be used for identifying **Resource** and **Link** elements in a HAL representation.

*   __name__

    OPTIONAL

    For distinguishing between **Resource** and **Link** elements that share the same **rel** value. The **name** attribute SHOULD NOT be used exclusively for identifying elements within a HAL representation, it is intended only as a 'secondary key' to a given **rel** value.

Note: the following attributes have corresponding [target attributes][9]' as defined in [Web Linking (RFC 5988)][7]

*   __hreflang__

    OPTIONAL

    For indicating what the language of the result of dereferencing the link should be.

*   __title__

    OPTIONAL

    For labeling the destination of a link with a human-readable identifier.

### Link Attributes

The following are attribute definitions applicable only to HAL's **Link** element.

*   __href__

    REQUIRED

    This attribute MAY contain a URI template. Whether or not this is the case SHOULD be indicated to clients by the __rel__ value.

### Resource Attributes

The following are attribute definitions applicable only to HAL's **Resource** element.

*    __href__

     REQUIRED

     Content embedded within a **Resource** element MAY be a full, partial, summary, or incorrect representation of the content available at the target URI. Applications which use HAL MAY clarify the integrity of specific embedded content via the description of the relevant **rel** value.

## Constraints

The root of a HAL representation MUST be a **Resource** with an href that corresponds to the effective request URI of the resource being represented.

## HAL in JSON (application/hal+json)

Note: click the following for a [more accessible explanation of HAL in JSON][3]

Further details on the JSON variant of HAL:

*   **Resources** are represented as objects
*   **Resource** objects have two reserved properties: \_links and \_embedded
*   The \_links property contains **Link** objects against keys that match their relation
*   The \_embedded property contains embedded **Resource** objects against keys that match their relation
*   **Resource** objects MUST have a self link (\_link.self) which indicates the URI of the embedded resource
*   Relations with one corresponding **Resource**/**Link** have a single object value, relations with multiple corresponding HAL elements have an array of objects as their value.

## Minimum Valid Representation

### JSON
{% highlight javascript %}
{ "_links": { "self": { "href": "http://example.com/" } } }
{% endhighlight %}

### XML
{% highlight xml %}
<resource href="http://example.com/" />
{% endhighlight %}

## Recommendations

### Using URIs for Link relation values

Link relation values used in HAL representations SHOULD be URIs which, when dereferenced, provide relevant details. This helps to improve the discoverability of your application.

For XML, the [CURIE syntax][10] MAY be used for brevity.

For JSON, a 'curie' link can be used like so:

{% highlight javascript %}
{ ... '_links' : { 'curie': { 'href' : 'http://example.com/rels/{relation}', 'name': 'ex' }, ... }, ... }
{% endhighlight %}

## Acknowledgements

Thanks to Darrel Miller and Mike Amundsen for their invaluable feedback.

## Notes/todo

Transclusion ala esi for JSON variant? XML can reuse ESI?

 [1]: mailto:mike%40stateless.co
 [2]: http://groups.google.com/group/hal-discuss
 [3]: http://blog.stateless.co/post/13296666138/json-linking-with-hal
 [4]: http://stateless.co/info-model.png
 [5]: http://tools.ietf.org/html/rfc2119
 [6]: http://tools.ietf.org/html/rfc5988#section-5.1
 [7]: http://tools.ietf.org/html/rfc5988
 [8]: http://tools.ietf.org/html/rfc5988#section-5.3
 [9]: http://tools.ietf.org/html/rfc5988#section-5.4
 [10]: http://www.w3.org/TR/curie/
 [11]: https://github.com/talios/halbuilder
 [12]: https://github.com/zircote/Hal
 [13]: http://hal.codeplex.com/
 [14]: https://bitbucket.org/smichelotti/hal-media-type
 [15]: https://github.com/blongden/hal
 [16]: https://github.com/apotonick/roar/blob/master/lib/roar/representer/json/hal.rb
