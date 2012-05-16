# DHAL - Dtime Hypertext Application Language

## A lean hypermedia type

HAL is a simple way of linking with JSON.

It provides a set of conventions for expressing hyperlinks to, and embeddedness of, related resources - the rest of a HAL document is just plain old JSON .
DHAL is a superset of hal adding support for data-templates.

This repo contains a formalised specification of DHAL (see [dhal\_specification.mkd][1]).

For a friendlier, more pracitcal introduction to HAL you can read this article: [JSON Linking with HAL][2].

## Discussion Group

If you have any questions or feedback about HAL, you can message the [HAL-discuss mailing list][3].

## Contributing
If you think some part of the spec needs changing, just fork this repo
and raise a pull request with your changes.

## Code
* [(Ruby) JSON::HAL][9]
* [(PHP) Hal Library][4]
* [(PHP) Nocarrier\Hal][8]
* [(C#) Hal.Net][5]
* [(C#) WCF Media Type Formatter][6]
* [(Java) HalBuilder][7]



 [1]: https://github.com/dtime/dhal_specification/blob/master/dhal_specification.mkd
 [2]: http://blog.stateless.co/post/13296666138/json-linking-with-hal
 [3]: http://groups.google.com/group/hal-discuss
 [4]: https://github.com/zircote/Hal
 [5]: https://github.com/talios/halbuilder
 [6]: http://hal.codeplex.com/
 [7]: https://bitbucket.org/smichelotti/hal-media-type
 [8]: https://github.com/blongden/hal
 [9]: https://github.com/apotonick/roar/blob/master/lib/roar/representer/json/hal.rb
