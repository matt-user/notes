# High Performance Browser Networking

## Foreword

* Good developers know how things work.  Great developers know why they work.

## 1. Primer on Latency and Bandwidth

* Latency - time from source to destination of a packet
* Bandwidth - throughput of comm path
* delay - propagation, transmission, processing, queuing
* propagation delay - time for signal to travel from source to destination
* transmission delay - time to push all bits into link
* processing delay - time to process packet at router
* queuing delay - time to wait at router
* user perceive lag at 100-200ms
* cdn help with this
* last mile latency

## 2. Building Blocks of TCP

* guarantees - in order and reliable bytes
* 3 way handshake
* TFO - tcp fast open - send data in first part of handshake
* congenstion collapse - round trip time exceeds retransmission timeout
* flow control - prevent sender from overwhelming receiver
    * tcp connection advertises receiver window
* congestion window - prevent network overload.  Sender side limit (slow start, congestion avoidance)
* SSR - slow start restart - resets congestion window
* bandwidth delay product - max amount of data in flight at any point

## 3. Building Blocks of UDP

* datagram - pretty much the same as packet
* no guarantee of message delivery, order, connection tracking, or congestion control
* NAT traversal - inability to establish udp connection between two hosts behind same NAT
* STUN - discover peers - keepalive pings
* TURN - relay peers

## 4. Transport Layer Security (TLS)

* SSL - oberserver can not read or modify the data, just observe its transmission
* uses encryption, authentication, and integrity
* handshake - negotiate encryption keys
* signs messages with MAC
* don't have to repeat full handshake for each connection - abbreviated handshake - start transmitting encrypted data before handshake completes
* session identifiers - server caches session for every client - or can use session tickets
* certificate authorities - verify certificate
* computation cost of encryption
* can optimize handshake with CDN

## 5. Introduction to wireless networks

* 