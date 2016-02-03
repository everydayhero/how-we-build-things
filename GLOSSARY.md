# Supporter
The consumer monorail.

# NFP
Not-for-profit. Depending on context, could be referring to the NFP product offering, or to the team that produces it.

# Universal
The same JS is executed by the client and server. For more detail, https://medium.com/@mjackson/universal-javascript-4761051b7ae9#.moyy8j83b

# Rails
http://rubyonrails.org/

# React
https://facebook.github.io/react/

# Ember
http://emberjs.com/

# Leader system
The sub-system acting as the authority for the data in question. It encapsulates all logic for this area of business capability, and is responsible for providing suitable access to all authorized consumers.

# Follower system
A sub-system which aggregates information from multiple leaders. A reporting system might, for example, maintain useful representations of data that cross many leader systems. JOINing customer-supplied metadata with fundraising page performance is a good example of this pattern.

# OLTP
On line transaction processing, or OLTP, is a class of information systems that facilitate and manage transaction-oriented applications, typically for data entry and retrieval transaction processing.

# OLAP
OLAP (online analytical processing) is computer processing that enables a user to easily and selectively extract and view data from different points of view. For example, a user can request that data be analyzed to display a spreadsheet showing all of a company's beach ball products sold in Florida in the month of July, compare revenue figures with those for the same products in September, and then see a comparison of other product sales in Florida in the same time period. To facilitate this kind of analysis, OLAP data is stored in a multidimensional database. Whereas a relational database can be thought of as two-dimensional, a multidimensional database considers each data attribute (such as product, geographic sales region, and time period) as a separate "dimension." OLAP software can locate the intersection of dimensions (all products sold in the Eastern region above a certain price during a certain time period) and display them. Attributes such as time periods can be broken down into subattributes.

# Semantic event
Business domain events, understandable outside of engineering, by people intimate with the way we deliver this area of business capability.

# Entities
A 'thing' with distinct and independent existence in our business. They tend to be a recurring noun. eg. a particular SupporterPage, Charity, User etc. If you can look them up using some kind of ID, they're an entity.

# PLAIN
Platform Lacking An Interesting Name

# Plain Utils
A console based toolkit for interacting with PLAIN. Used for deploying + managing apps, etc.

# Docker
A tool for deploying software inside of isolated containers.

# Mesos
A cluster management system that abstracts compute resources away from underlying system using Docker.

# Marathon
A scheduling and control system for services running on top of Apache Mesos.

# Consul
A distributed, multi datacenter service discovery mechanism with a key value store.

# Terraform
A tool for describing infrastructure as code. Handles lower level

# Ansible
Configuration management tool that handles OS/Application layer management.

# Kibana
A data visualization plugin for Elasticsearch.

# Elasticsearch
A search server based on Lucene. Provides a distributed full-text search engine with a REST/JSON API.

# Statsd

# Admin
TODO

# Cheese
Food for the ROUS (Event Gateway API for the Reporting System)

# Larder
Event storage mechanism for Cheese to feed the ROUS using an S3 backend.

# Core
TODO

# Eventbrite
Custom integration for the [Eventbrite](https://www.eventbrite.com) ticket management platform.

# Heroix
Heroix 3 is a legacy charity campaign administration & reporting interface for AU/NZ/UK that also handles donations for these regions. Superseded by NFP.

# Heroix Microsites
A proxy frontend to statically generated campaign microsites.

# Mad Max
Receipt generator service used by Payments

# Mailroom
The transaction email engine used by NFP. Made up of 3 components: Deliverator, Validator and Renderer.

# MDS
Metadata service.

# NFP Profile
An isomorphic SPA for NFP Charity Profiles

# Panoramix
A service that produces reports and provides data API endpoints from across our business.

# Payments
Depending on context, Payments either refers to the payment processing system or the team that produces it.

# Primocache
A cache service for the Primo Financials API

# ROUS
Rodents of Unusual Size. Provides reporting and dashboards for NFP

# Supporter
The main consumer facing peer-to-peer platform.

# The Thing
Provides a HTTP API that exposes all charities in a given campaign regardless of state to Payments.

# Two Face
Everyday Hero Public API Facade

# Vanity URL
Provides vanity urls for [WalkTheWalk](http://www.wtwalk.org) campaigns.

# SPA
Single Page Application

# Zuul
A standalone service responsible for serving, validating and processing donations that can withstand both upstream and downstream outages.

# Kraken
An automated provisioning service built on Terraform and Ansible to handle AWS Autoscaling.
