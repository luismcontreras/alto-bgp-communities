---
coding: utf-8

title: Extending ALTO by using BGP Communities
abbrev: Extending ALTO by using BGP Communities
docname: draft-contreras-alto-bgp-communities-01
category: info

standalone: yes
pi: [toc, sortrefs, symrefs, comments]

ipr: trust200902
area: Routing
wg: ALTO
kw: ALTO, PID, BGP Communities

# {::boilerplate bcp14}

author: 

  -
    fullname: Luis M. Contreras
    organization: Telefonica
    street: Ronda de la Comunicacion, s/n
    city: Madrid
    code: 28050
    country: Spain
    email: luismiguel.contrerasmurillo@telefonica.com
    uri: http://lmcontreras.com
	
normative:

informative:


--- abstract

This memo introduces a proposal to extend ALTO by using BGP Communities as PIDs. This proposal is meant to ease the integration of ALTO 
in operational networks by leveraging existing resource identifiers.

--- middle

# Introduction

The Provider-defined Identifiers (PIDs) in the ALTO Protocol {{?RFC7285}} provide an indirect and network-agnostic way to aggregate 
a set of network endpoints, that grouped together creates a network map. Network endpoints that share a common PID are expected to receive 
similar treatment on the decisions assisted by ALTO.

With the same goal of grouping destinations, BGP Communities {{?RFC1997}} were introduced in the past to tag a grouping of destinations 
so that the routing decision can also be based on the identity of a group. As per {{?RFC1997}}, a community is a group of destinations 
which share some common properties.

Given that BGP communities are widely used in operational networks, and for the sake of simplifying the integration of ALTO into these 
networks, this document specifies an extension to {{?RFC7285}} by defining a new PID type based on the BGP community concept.

# BGP Communities Overview

A BGP Community, as per {{?RFC1997}}, is a BGP attribute that is used to group destinations. 
It is represented as a 32-bit integer number, typically written as the combination of two 16-bit integer numbers
separated by a colon. The first number is usually the Autonomous System (AS) number, while the second one is determined by the 
service provider according to some internal logic. In order to support 4-octet ASNs, {{?RFC8092}} specifies a BGP Large Communities 
attribute. Another form of BGP communities is defined in the BGP Extended Communities Attribute {{?RFC4360}}. IP prefixes can be part of 
distinct BGP Communities, with different purposes, typically to influence the traffic reaching the particular prefixes of a community.

The BGP Communities attribute is useful for applying policies of applicability to a certain set of prefixes, grouped as a community 
at the criteria of the service provider. For instance, BGP Communities can be useful for indicating local preferences 
for a route to the receive to a set of IP prefixes in a peering scenario.

The initial approach in the usage of BGP Communities in ALTO that we follow in this document is to consider {{?RFC1997}} and {{?RFC8092}} as the means
to identify groupings of IP prefixes in networks with either 2-octet or 4-octet ASNs.

# Extending ALTO with BGP communities

Network operators use BGP Communities extensively as a mechanism to group some destinations (i.e., IP prefixes) for different purposes. 
Typically, they are used by administratively-defined filters for applying policies, thus influencing the behavior of the traffic towards 
the associated destinations.

On the other hand, the ALTO Protocol is based on IP prefixes. When considering queries to IP prefixes, it could be the case that those 
queries apply to IP addresses associated to the same topological element.  This is for instance the case of aggregation nodes in the 
network (such as BNG or PGW), which have associated a number of IP prefixes (that can evolve along the time). The same response will be 
obtained from an ALTO server for all the prefixes associated with such a node, since the topological information is essentially the same.

For assisting an efficient usage of ALTO resources in this kind of situations, the usage of BGP Communities simplifies the process by 
reducing the number of queries to the ALTO server, but also by smoothly absorbing the modification of prefixes for a given aggregation node.

## Usage of BGP Communities in ALTO

Some potential use cases of BGP Communities in ALTO are envisaged:

* In situations where a BGP Community and an ALTO PID scope the same grouping of prefixes, leveraging BGP Communities simplifies 
network operations by using an existing identifier for the purpose of retrieving ALTO information.

* In situations where the purpose is to retrieve ALTO information applicable to a superset of PIDs, a BGP Community can be defined in order
to group the prefixes of all those PIDs.

* In situations where the purpose is to retrieve ALTO information applicable to a subset of prefixes across multiple PIDs, a BGP Community
can be defined in order to group the subset of prefixes of all the PIDs.

## BGP Community representation in ALTO

To be done.

# Security Considerations

To be provided.
  
# IANA Considerations

To be provided.

# Acknowledgements

The author thanks Med Boucadair for his review, comments and suggestions to make this document and solution more complete.