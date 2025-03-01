# High Performance Browser Networking

## Foreword

* Good developers know how things work.  Great developers know why they work.

## 1. Primer on Latency and Bandwidth

* Latency - time from source to destination of a packet
* Bandwidth - throughput of comm path
* delay - propagation, transmission, processing, queuing
* user perceive lag at 100-200ms
* cdn help with this
* last mile latency

## Building Blocks of TCP

* guarantees - in order and reliable bytes
* 3 way handshake
* TFO - tcp fast open - send data in first part of handshake
* congenstion collapse - round trip time exceeds retransmission timeout
* flow control - prevent sender from overwhelming receiver
    * tcp connection advertises receiver window
* congestion window - prevent network overload.  Sender side limit (slow start, congestion avoidance)
* SSR - slow start restart - resets congestion window
* bandwidth delay product - max amount of data in flight at any point

## Building Blocks of UDO

* datagram - pretty much the same as packet
* no guarantee of message delivery, order, connection tracking, or congestion control
* STUN - discover peers
* TURN - relay peers

