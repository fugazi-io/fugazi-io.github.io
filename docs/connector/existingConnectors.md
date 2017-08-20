# Existing Connectors

The following is a list of existing implementations of fugazi connectors for different services which do not expose 
an http based api.  
All of these projects use the base [node connector package](https://github.com/fugazi-io/connector.node) which 
implements the basic needs for a fugazi connector.
These project can also be used as examples of how to use the connector node package to create your own connector.

* [Redis connector](https://github.com/fugazi-io/connector.node.redis): Exposes a [redis](https://redis.io/) client.
* [MongoDB connector](https://github.com/fugazi-io/connector.node.mongo): Exposes a [mongo](https://www.mongodb.com/) client.
* [SSH connector](https://github.com/fugazi-io/connector.node.ssh): Adds the ability to execute 
[ssh](https://en.wikipedia.org/wiki/Secure_Shell) commands.

This list will be updated when more connectors are implemented.  
If you've created a connector which can be useful for others, please [contact us](../#?id=contact-help-amp-contribute ":target=_self") 
so we'll add it to the list.