ruby-mqttx
=========

Pure Ruby gem that implements the [MQTT] protocol, a lightweight protocol for publish/subscribe messaging.

This fork is mainly based for the support of QOS=1 and QOS=2.

## Table of Contents ##
- [Installation](#installation)
- [MQTT Protocol Summary](#mqtt-protocol)
- [Library Usage](#synopsis)
- [Resources](#resources)
- [License](#license)
- [Contact](#contact)

Installation
------------

You can obtain the .gem file and install with:

```
   $ gem install mqttx-X.X.X.gem
```

Optionally, you can use this fork using bundler:
```
    gem 'mqttx', :git => 'https://github.com/tierconnect/ruby-mqtt.git'
```

or require it with:

```ruby
   require 'mqttx'
```

## MQTT Protocol ##


MQTT is a lightweight messaging protocol based on message publishing/subscription, for use on top of a TCP/IP protocol. Every MQTT message includes a topic that classifies it. MQTT servers use topics to determine which subscribers should receive messages published to the server.

To provide more flexibility, MQTT supports a hierarchical topic namespace. This allows app designers to organize topics to simplify their management. Levels in the hierarchy are delimited by the '/' character.


### Wildcards ###

For subscriptions, two wildcard characters are supported:

* A '#' character represents a complete sub-tree of the hierarchy, and thus, must be the last character in a subscription topic string, such as SENSOR/#. This will match any topic starting with SENSOR/, such as SENSOR/1/TEMP and SENSOR/2/HUMIDITY.

* A '+' character represents a single level of the hierarchy and is used between delimiters. For example, SENSOR/+/TEMP will match SENSOR/1/TEMP and SENSOR/2/TEMP.

Publishers are not allowed to use the wildcard characters in their topic names.


### QoS ###

Quality of Service is a networking term that specifies a guaranteed throughput level. Quality of service technology is intended for guaranteed timely delivery of specific application data or resources to a particular destination or destinations.

Different levels of QoS are used in this ruby-mqtt interface:

* QoS Level 0 - where a message is sent by the user to the server and all those currently connected to it. No confirmation of reception is returned to the user.
* QoS Level 1 - where the message is sent under the same conditions described for QoS Level 0, and a reception acknowledgement is issued by the server.
* QoS Level 2 - where a message is sent to the server requesting acknnowledgement of readiness for reception. The server responds indicating its readiness to receive the message. Afterwards, the same conditions apply as for QoS Level 1.

Functionality does not change while using any of the 3 above-mentioned conditions. However, a higher level of QoS is oriented towards higher reliability and will, consequently, result in a slight decrease in speed.


### Retain messages ###

The use of this capability enables the user to mark a message to be retained for future use and users. The system retains the message and publishes it for each subsequent subscriber. There is a limit of 1 retained message per user per topic!!!


### Clean/Unclean sessions ###

A clean session implies beginning a new session from scratch. When you connect an MQTT client application using the MqttClient.connect method, the client identifies the connection using the client identifier and the address of the server. The server checks whether session information has been saved from a previous connection to the server. If a previous session still exists, and cleanSession=true, then the previous session information at the client and server is cleared. If cleanSession=false the previous session is resumed. If no previous session exists, a new session is started.

In an unclean or "dirty" session, the user specifies whether to continue the last session saved. Otherwise, the system will launch a brand new session. A previously-saved session is related to the user's subscription and it will save the subscription and all saved messages related to that subscription if QoS>0.

Upon opening of an unclean session, all messages received while the user was disconnected from the system will appear.


### Will message ###

<<<<<<< HEAD
If a MQTT client connection ends unexpectedly, the user can configure mqtt to send a "last will and testament" message. The content of the message must be predefined, as well as the topic to send it to. The "last will" is a connection property. It must be created before connecting the client.

The will message is comprised of a topic, payload, QoS level, and a retain value.
=======
Table of Contents
-----------------
* [Installation](#installation)
* [Quick Start](#quick-start)
* [Library Overview](#library-overview)
* [Resources](#resources)
* [License](#license)
* [Contact](#contact)


Installation
------------

You may get the latest stable version from [Rubygems]:
>>>>>>> master


Alternatively, to use a development snapshot from GitHub using [Bundler]:

    gem 'mqtt', :git => 'https://github.com/njh/ruby-mqtt.git'

<<<<<<< HEAD
## Synopsis ##

=======

Quick Start
-----------
>>>>>>> master

    require 'rubygems'
    require 'mqtt'

    # Publish example
    MQTT::Client.connect('test.mosquitto.org') do |c|
      c.publish('topic', 'message')
    end

    # Subscribe example
    MQTT::Client.connect('test.mosquitto.org') do |c|
      # If you pass a block to the get method, then it will loop
      c.get('test') do |topic,message|
        puts "#{topic}: #{message}"
      end
    end

Connection:

    client = MQTT::Client.connect('mqtt://myserver.example.com')
    client = MQTT::Client.connect('mqtt://user:pass@myserver.example.com')
    client = MQTT::Client.connect('myserver.example.com')
    client = MQTT::Client.connect('myserver.example.com', 18830)
    client = MQTT::Client.connect({:remote_host => 'myserver.example.com',:remote_port => 1883 ... })

SSL Connection

    client = MQTT::Client.new({:remote_host => 'myserver.example.com',:remote_port => 1883,:ssl => true })
    client.cert_file = path_to('client.pem')
    client.key_file  = path_to('client.key')
    client.ca_file   = path_to('root-ca.pem')
    client.connect()

The connection can be made without the use of a block:

    client = MQTT::Client.connect('myserver.example.com', 18830)
       #client stuff
    client.disconnect()

Or, if using a block, with an implicit disconnection at the end of the block.

    MQTT::Client.connect('myserver.example.com', 18830) do |client|
       #client stuff
    end


The default options for the map parameter are:

    ATTR_DEFAULTS = {
       :remote_host => nil,
       :remote_port => nil,

       :keep_alive => 15,
       :clean_session => true,
       :client_id => nil,
       :ack_timeout => 5,
       :username => nil,
       :password => nil,

       :will_topic => nil,
       :will_payload => nil,
       :will_qos => 0,
       :will_retain => false,

       :reconnect => false,
       :ssl => false,
       :protocol_version  => :v31
    }

* :keep_alive - Time to determine a live client/server
* :clean_session - Start with a new session or an unclean one (subscriptions wiped or saved, respectively)
* :client_id - Client id sent to the server. Autogenerated if nil.
* :ack_timeout - Timeout to receive an ACK.
* :username - If the servers supports auth, the username.
* :password - If the servers supports auth, the username.

A will topic is a message that the server should send in the scenario of a client disconnection.
* :will_topic
* :will_payload
* :will_qos
* :will_retain

Miscellanea parameters.

* :reconnect - If the connection is dropped, reconnect.
* :ssl - If the connection will use a ssl connection
* :protocol_version By default, messages are sent complying with the MQTT 3.1.0 spec. With this parameter set as `:v31`, the messages are sent based on the MQTT 3.1.1 spec. If you want to use the 3.1.1 MQTT spec, you need to set this value to `:v311`

Subscribe
Select topic and qos level (0 if not provided)

    client.subscribe( 'a/b' )
    client.subscribe( 'a/b', 'c/d' )
    client.subscribe( ['a/b',0], ['c/d',1] )
    client.subscribe( 'a/b' => 0, 'c/d' => 1 )

Unsubscribe

    client.unsubscribe('topic1','topic2','topic3')

Publish

    client.publish(topic, payload, retain=false, qos=0)

Get Messages

    client.get(topic) do |topic,message|

    end

or

    topic,message = client.get(topic)


Library Overview
----------------

### Connecting ###

A new client connection can be created by passing either a [MQTT URI], a host and port or by passing a hash of attributes.

    client = MQTT::Client.connect('mqtt://myserver.example.com')
    client = MQTT::Client.connect('mqtts://user:pass@myserver.example.com')
    client = MQTT::Client.connect('myserver.example.com')
    client = MQTT::Client.connect('myserver.example.com', 18830)
    client = MQTT::Client.connect(:host => 'myserver.example.com', :port => 1883 ... )

TLS/SSL is not enabled by default, to enabled it, pass ```:ssl => true```:

    client = MQTT::Client.connect(
      :host => 'test.mosquitto.org',
      :port => 8883
      :ssl => true
    )

Alternatively you can create a new Client object and then configure it by setting attributes. This example shows setting up client certificate based authentication:

    client = MQTT::Client.new
    client.host = 'myserver.example.com'
    client.ssl = true
    client.cert_file = path_to('client.pem')
    client.key_file  = path_to('client.key')
    client.ca_file   = path_to('root-ca.pem')
    client.connect()

The connection can either be made without the use of a block:

    client = MQTT::Client.connect('test.mosquitto.org')
    # perform operations
    client.disconnect()

Or, if using a block, with an implicit disconnection at the end of the block.

    MQTT::Client.connect('test.mosquitto.org') do |client|
      # perform operations
    end
    
For more information, see and list of attributes for the [MQTT::Client] class and the [MQTT::Client.connect] method.


### Publishing ###

To send a message to a topic, use the ```publish``` method:

    client.publish(topic, payload, retain=false)

The method will return once the message has been sent to the MQTT server.

For more information see the [MQTT::Client#publish] method.


### Subscribing ###

You can send a subscription request to the MQTT server using the subscribe method. One or more [Topic Filters] may be passed in:

    client.subscribe( 'topic1' )
    client.subscribe( 'topic1', 'topic2' )
    client.subscribe( 'foo/#' )

For more information see the [MQTT::Client#subscribe] method.


### Receiving Messages ###

To receive a message, use the get method. This method will block until a message is available. The topic is the name of the topic the message was sent to. The message is a string:

    topic,message = client.get

Alternatively, you can give the get method a block, which will be called for every message received and loop forever:

    client.get do |topic,message|
      # Block is executed for every message received
    end

For more information see the [MQTT::Client#get] method.



Limitations
-----------

<<<<<<< HEAD
 * ~~Only QOS 0 is currently supported~~
=======
 * Only QOS 0 currently supported
 * Automatic re-connects to the server are not supported
>>>>>>> master


Resources
---------

* API Documentation: http://rubydoc.info/gems/mqtt
* Protocol Specification v3.1.1: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.html
* Protocol Specification v3.1: http://public.dhe.ibm.com/software/dw/webservices/ws-mqtt/mqtt-v3r1.html
* MQTT Homepage: http://www.mqtt.org/
* GitHub Project: http://github.com/njh/ruby-mqtt


License
-------

The ruby-mqtt gem is licensed under the terms of the MIT license.
See the file LICENSE for details.


Contact
-------

* Author:    Nicholas J Humfrey
* Email:     njh@aelius.com
* Home Page: http://www.aelius.com/njh/



[MQTT]:           http://www.mqtt.org/
[Rubygems]:       http://rubygems.org/
[Bundler]:        http://bundler.io/
[MQTT URI]:       https://github.com/mqtt/mqtt.github.io/wiki/URI-Scheme
[Topic Filters]:  http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.html#_Toc388534397

[MQTT::Client]:           http://rubydoc.info/gems/mqtt/MQTT/Client#instance_attr_details
[MQTT::Client.connect]:   http://rubydoc.info/gems/mqtt/MQTT/Client.connect
[MQTT::Client#publish]:   http://rubydoc.info/gems/mqtt/MQTT/Client:publish
[MQTT::Client#subscribe]: http://rubydoc.info/gems/mqtt/MQTT/Client:subscribe
[MQTT::Client#get]:       http://rubydoc.info/gems/mqtt/MQTT/Client:get

