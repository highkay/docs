.. _ofconfig-dev-guide:

OF-CONFIG Developer Guide
=========================

Overview
--------

OF-CONFIG defines an OpenFlow switch as an abstraction called an
OpenFlow Logical Switch. The OF-CONFIG protocol enables configuration of
essential artifacts of an OpenFlow Logical Switch so that an OpenFlow
controller can communicate and control the OpenFlow Logical switch via
the OpenFlow protocol. OF-CONFIG introduces an operating context for one
or more OpenFlow data paths called an OpenFlow Capable Switch for one or
more switches. An OpenFlow Capable Switch is intended to be equivalent
to an actual physical or virtual network element (e.g. an Ethernet
switch) which is hosting one or more OpenFlow data paths by partitioning
a set of OpenFlow related resources such as ports and queues among the
hosted OpenFlow data paths. The OF-CONFIG protocol enables dynamic
association of the OpenFlow related resources of an OpenFlow Capable
Switch with specific OpenFlow Logical Switches which are being hosted on
the OpenFlow Capable Switch. OF-CONFIG does not specify or report how
the partitioning of resources on an OpenFlow Capable Switch is achieved.
OF-CONFIG assumes that resources such as ports and queues are
partitioned amongst multiple OpenFlow Logical Switches such that each
OpenFlow Logical Switch can assume full control over the resources that
is assigned to it.

How to start
------------

-  start OF-CONFIG feature as below:

   ::

       feature:install odl-of-config-all

Compatible with NETCONF
-----------------------

-  Config OpenFlow Capable Switch via OpenFlow Configuration Points

   Method: POST

   URI:
   http://localhost:8181/restconf/config/network-topology:network-topology/topology/topology-netconf/node/controller-config/yang-ext:mount/config:modules

   Headers: Content-Type" and "Accept" header attributes set to
   application/xml

   Payload:

   .. code:: xml

       <module xmlns="urn:opendaylight:params:xml:ns:yang:controller:config">
         <type xmlns:prefix="urn:opendaylight:params:xml:ns:yang:controller:md:sal:connector:netconf">prefix:sal-netconf-connector</type>
         <name>testtool</name>
         <address xmlns="urn:opendaylight:params:xml:ns:yang:controller:md:sal:connector:netconf">10.74.151.67</address>
         <port xmlns="urn:opendaylight:params:xml:ns:yang:controller:md:sal:connector:netconf">830</port>
         <username xmlns="urn:opendaylight:params:xml:ns:yang:controller:md:sal:connector:netconf">mininet</username>
         <password xmlns="urn:opendaylight:params:xml:ns:yang:controller:md:sal:connector:netconf">mininet</password>
         <tcp-only xmlns="urn:opendaylight:params:xml:ns:yang:controller:md:sal:connector:netconf">false</tcp-only>
         <event-executor xmlns="urn:opendaylight:params:xml:ns:yang:controller:md:sal:connector:netconf">
           <type xmlns:prefix="urn:opendaylight:params:xml:ns:yang:controller:netty">prefix:netty-event-executor</type>
           <name>global-event-executor</name>
         </event-executor>
         <binding-registry xmlns="urn:opendaylight:params:xml:ns:yang:controller:md:sal:connector:netconf">
           <type xmlns:prefix="urn:opendaylight:params:xml:ns:yang:controller:md:sal:binding">prefix:binding-broker-osgi-registry</type>
           <name>binding-osgi-broker</name>
         </binding-registry>
         <dom-registry xmlns="urn:opendaylight:params:xml:ns:yang:controller:md:sal:connector:netconf">
           <type xmlns:prefix="urn:opendaylight:params:xml:ns:yang:controller:md:sal:dom">prefix:dom-broker-osgi-registry</type>
           <name>dom-broker</name>
         </dom-registry>
         <client-dispatcher xmlns="urn:opendaylight:params:xml:ns:yang:controller:md:sal:connector:netconf">
           <type xmlns:prefix="urn:opendaylight:params:xml:ns:yang:controller:config:netconf">prefix:netconf-client-dispatcher</type>
           <name>global-netconf-dispatcher</name>
         </client-dispatcher>
         <processing-executor xmlns="urn:opendaylight:params:xml:ns:yang:controller:md:sal:connector:netconf">
           <type xmlns:prefix="urn:opendaylight:params:xml:ns:yang:controller:threadpool">prefix:threadpool</type>
           <name>global-netconf-processing-executor</name>
         </processing-executor>
       </module>

-  NETCONF establishes the connections with OpenFlow Capable Switches
   using the parameters in the previous step. NETCONF also gets the
   information of whether the OpenFlow Switch supports NETCONF during
   the signal handshaking. The information will be stored in the NETCONF
   topology as prosperity of a node.

-  OF-CONFIG can be aware of the switches accessing and leaving by
   monitoring the data changes in the NETCONF topology. For the detailed
   information it can be refered to the
   `implementation <https://git.opendaylight.org/gerrit/gitweb?p=of-config.git;a=blob_plain;f=southbound/southbound-impl/src/main/java/org/opendaylight/ofconfig/southbound/impl/OdlOfconfigApiServiceImpl.java;hb=refs/heads/stable/boron>`__.

The establishment of OF-CONFIG topology
---------------------------------------

Firstly, OF-CONFIG will check whether the newly accessed switch supports
OF-CONFIG by querying the NETCONF interface.

1. During the NETCONF connection’s establishment, the NETCONF and the
   switches will exchange the their capabilities via the "hello"
   message.

2. OF-CONFIG gets the connection information between the NETCONF and
   switches by monitoring the data changes via the interface of
   DataChangeListener.

3. After the NETCONF connection established, the OF-CONFIG module will
   check whether OF-CONFIG capability is in the switch’s capabilities
   list which is got in step 1.

4. If the result of step 3 is yes, the OF-CONFIG will call the following
   processing steps to create the topology database.

For the detailed information it can be referred to the
`implementation <https://git.opendaylight.org/gerrit/gitweb?p=of-config.git;a=blob_plain;f=southbound/southbound-impl/src/main/java/org/opendaylight/ofconfig/southbound/impl/listener/OfconfigListenerHelper.java;hb=refs/heads/stable/boron>`__.

Secondly, the capable switch node and logical switch node are added in
the OF-CONFIG topology if the switch supports OF-CONFIG.

OF-CONFIG’s topology compromise: Capable Switch topology (underlay) and
logical Switch topology (overlay). Both of them are enhanced (augment)
on

/topo:network-topology/topo:topology/topo:node

The NETCONF will add the nodes in the Topology via the path of
"/topo:network-topology/topo:topology/topo:node" if it gets the
configuration information of the switches.

For the detailed information it can be referred to the
`implementation <https://git.opendaylight.org/gerrit/gitweb?p=of-config.git;a=blob;f=southbound/southbound-api/src/main/yang/odl-ofconfig-topology.yang;h=dbdaec46ee59da3791386011f571d7434dd1e416;hb=refs/heads/stable/boron>`__.

