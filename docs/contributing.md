# Contributing to fugazi.io

fugazi.io is made up of a several projects:
 * [webclient](https://github.com/fugazi-io/webclient): The web terminal which is served from http://fugazi.io
 * [proxify](https://github.com/fugazi-io/proxify): A node package for turning any http based API into a fugazi module which can be loaded and used from the terminal
 * [connector.node](https://github.com/fugazi-io/connector.node): A node package for connecting node based applications to the terminal
 * [connector.node.redis](https://github.com/fugazi-io/connector.node.redis): A node package, based on the connector.node, for connecting redis to the terminal, creating a redis cli client
 * [connector.node.mongo](https://github.com/fugazi-io/connector.node.mongo): A node package, based on the connector.node, for connecting MongoDB to the terminal, creating a mongo cli client 
 * [connector.node.ssh](https://github.com/fugazi-io/connector.node.ssh): A node package, based on the connector.node, for executing ssh commands 

While the main project (_webclient_) is big and complicated, the smaller ones are very easy to understand and work with without previous experience.  
If you wish to contribute to the _webclient_ project, then check out the [list of existing issues](https://github.com/fugazi-io/webclient/issues).  

The _mongo_ and _redis_ project could use help with implementing more commands.  
It's a pretty simple task to implement a command in one of these two projects, a more detailed description can be found here:
 * [Add a Redis command](https://github.com/fugazi-io/connector.node.redis/wiki/Add-a-Redis-command)
 * [Add a MongoDB command](https://github.com/fugazi-io/connector.node.mongo/wiki/Add-a-MongoDB-command)

Creating new [connectors](connector ":target=_self") can be easy using the **connector.node** package, 
while the **connector.node.redis**, **connector.node.mongo** and **connector.node.ssh** packages can be used as 
examples of how to do so.

We'd also love to have a connector to the [wordpress rest api](https://developer.wordpress.org/rest-api/), a more 
detailed description can be found here: [wordpress rest api connector](https://github.com/fugazi-io/webclient/issues/77).

Contact us [in gitter](https://gitter.im/fugazi-io/Lobby) for more help and information.