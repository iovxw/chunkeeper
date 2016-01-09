Persistent reliable distributed storage with consideration to fairness

## Chunkeeper Usage Example ##
```
// init the module
Injector injector = Guice.createInjector(
    new KadNetModule()
        .setProperty("openkad.net.udp.port", "5555"),

    new HttpConnectorModule()
        .setProperty("httpconnector.net.port", "5555"),
					
    new SimpleDHTModule(),
						
    new ChunKeeperModule()
);

// initiate the key based routing network		
KeybasedRouting kbr = injector.getInstance(KeybasedRouting.class);
kbr.create();

// initiate http communication to be used by the chunkeeper			
HttpConnector httpConnector = injector.getInstance(HttpConnector.class);
httpConnector.bind();
httpConnector.start();

// initiate the chunkeeper	
ChunKeeper chunkeeper = injector.getInstance(ChunKeeper.class);
chunkeeper.bind();

// join the network
// format of the uri: [protocol]://[address:port]/
kbr.join(new URI("openkad.udp://1.2.3.4:5555/"));

// store some data !

// generate an arbitrary chunk key
KeyFactory kf = kbr.getKeyFactory();
Key k1 = kf.create("some arbitrary tag to be used for finding the stored chunk");

// map some data to key k1
chunkeeper.store(k1, "data");


// retrieve the chunk (can be done by any other node in the network)
Set<Chunk> chunks = chunkeeper.findChunk(k1);

// download and print all chunks
for (Chunk chunk : chunks) {
    System.out.println(chunk.download());
}

```




## E-Wolf Project ##
  * [openkad](http://code.google.com/p/openkad/)
  * [dht](http://code.google.com/p/dht/)
  * [chunkeeper](http://code.google.com/p/chunkeeper/)
  * [social-fs](http://code.google.com/p/social-fs/)
  * [ewolf](http://code.google.com/p/ewolf/)
  * [postman](https://code.google.com/p/postman-pubsub/)