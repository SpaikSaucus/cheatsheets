[![en](https://img.shields.io/badge/lang-en-red.svg):ballot_box_with_check:](#) [![es](https://img.shields.io/badge/lang-es-yellow.svg):black_large_square:](https://github.com/SpaikSaucus/cheatsheets/blob/main/DataBase/Redis/README.es.md)

# CheatSheets Redis

## Table of Contents
- [What is Redis?](#what-is-redis)
- [What is a Distributed cache?](#what-is-a-distributed-cache)
- [Advantages of cache](#advantages-of-cache)
- [Redis Sentinel vs Cluster](#redis-sentinel-vs-cluster)

## What is Redis?
It is a database engine, usually used as an in-memory database that works under the key-value scheme, it can be used as a cache, message queue, session manager, etc. It is written in ANSI C and is offered as an open source solution (BSD License) by Redis Labs.
Redis also incorporates a Lua interpreter and scales horizontally under the master-slave philosophy. Allowing a slave to be a master for others. It offers client libraries for the most popular languages on the market.

## What is a Distributed cache?
A distributed cache is a cache shared by multiple application servers, typically maintained as an external service to the application servers that have access to it.

## Advantages of cache
We will mention some of the advantages:
* Improved application performance.
     * You do not have to be consulting data access sources at all times.
* Allows you to increase throughput.
     * The number of requests that are processed per unit of time, if we give smaller response times we increase the number of operations we perform per unit of time.
* Reduction of processing on the DB side.
* Predictable performance.
     * The results are previously saved and access is much faster than accessing a resource in the DB
* Session Control.

## Redis Sentinel vs Cluster

__Redis Sentinel__: High availability solution with 3 nodes (1 primary and 2 secondary). The connection to the base is made by consulting the sentinel agent on which node the primary is located.

__Redis Cluster__: High availability solution with 6 nodes (3 primary and 3 secondary). The data is divided between the 3 primary nodes, distributing the load.