Ruby MQTT NEWS
==============

Ruby MQTT Version 0.2.0 (2014-04-02)
------------------------------------

* Added SSL/TLS support
* Added support for passing connection details using a URI
* Added support for using the MQTT_BROKER environment variable
* Allow passing array of topics to Client#unsubscribe
* Allow more combinations of arguments to be passed to a new Client
* No longer defaults to ‘localhost’ if there is no broker configured
* Fixed more 'unused variable' warnings
* Documentation improvements
* Ruby 1.8 fixes
* Ruby 2 fixes


Ruby MQTT Version 0.1.0 (2013-09-07)
------------------------------------

* Changed license to MIT, to simplify licensing concerns
* Improvements for UTF-8 handling under Ruby 1.9
* Added ```get_packet``` method
* Added support for a keep-alive value of 0
* Added a #inspect method to the Packet classes
* Added checks for the protocol name and version
* Added check to ensure that packet body isn't too big
* Added validation of QoS value
* Added example of using authentication
* Fixed 'unused variable' warnings
* Reduced duplicated code in packet parsing
* Improved testing
  - Created fake server and integration tests
  - Better test coverage
  - Added more tests for error states


Ruby MQTT Version 0.0.9 (2012-12-21)
------------------------------------

* Fixes for Ruby 1.9.3 by Mike English
* Fix for ```client_id``` typo by Anubisss
* Added methods to inspect the incoming message queue: ```queue_empty?``` and ```queue_length```
* Fixed incorrect implementation of the parsing and serialising of Subscription Acknowledgement packets
* Changed test mocking from Mocha to rspec-mocks


Ruby MQTT Version 0.0.8 (2011-02-04)
------------------------------------

* Implemented Last Will and Testament feature
* Renamed dup attribute to duplicate to avoid method name clash
* Made the random ```client_id``` generator a public class method


Ruby MQTT Version 0.0.7 (2011-01-19)
------------------------------------

* You can now pass a topic and block to client.get
* Added MQTT::Client.connect class method


Ruby MQTT Version 0.0.5 (2011-01-18)
------------------------------------

* Implemented setting username and password (MQTT 3.1)
* Renamed ```clean_start``` to ``clean_session```
* Started using autoload to load classes
* Modernised Gem building mechanisms


Ruby MQTT Version 0.0.4 (2009-02-22)
------------------------------------

* Re-factored packet encoding/decoding into one class per packet type
* Added MQTT::Proxy class for implementing an MQTT proxy


Ruby MQTT Version 0.0.3 (2009-02-08)
------------------------------------

* Added checking of Connection Acknowledgement
* Automatic client identifier generation


Ruby MQTT Version 0.0.2 (2009-02-03)
------------------------------------

* Added support for packets longer than 127 bytes


Ruby MQTT Version 0.0.1 (2009-02-01)
------------------------------------

* Initial Release
