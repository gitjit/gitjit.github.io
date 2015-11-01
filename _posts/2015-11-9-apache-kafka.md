---
layout: post
title:  "Kafka Introduction"
date:   2015-10-29 09:45:36
categories: kafka hadoop
author : Jithesh Chandrasekharan
---

Kafka is a distributed, partitioned, replicated commit log service. It provides the functionality of a messaging system, but with a unique design.
What does all that mean?

First let's review some basic messaging terminology:

Kafka maintains feeds of messages in categories called topics.
We'll call processes that publish messages to a Kafka topic producers.
We'll call processes that subscribe to topics and process the feed of published messages consumers..
Kafka is run as a cluster comprised of one or more servers each of which is called a broker.
So, at a high level, producers send messages over the network to the Kafka cluster which in turn serves them up to consumers like this: