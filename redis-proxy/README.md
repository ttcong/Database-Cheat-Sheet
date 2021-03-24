Still oldschool Redis Replication (Master - Read Replicas) but real High Avaibility:
+ Sentinel (at least 3 node): same host as each Redis-Server, automatic failover for Redis Replication

     
              +----+
              | M1 |
              | S1 |
              +----+
                 |
                 |
       +----+    |    +----+
       | R2 |----+----| R3 |
       | S2 |         | S3 |
       +----+         +----+

      Configuration: quorum = 2


+ HAProxy for Read/Write Spliting (2 endpoints): SET will auto routed to Master, GET auto routed to Read Replicas.
+ VIP + Keepalived: Failover for HaProxy.
+ HAProxy/VIP/Keepalived can be on same host as Sentinel/Redis or not.

              +----+
              | H1 |
              | K1 |
              | M1 |
              | S1 |
              +----+
                 |
                 |
       +----+    |    +----+
       | H2 |    |    | H3 |
       | K2 |    |    | K3 |
       | R2 |----+----| R3 |
       | S2 |         | S3 |
       +----+         +----+
       
  or
  
                VIP
              /      \
       +----+    |    +----+
       | H1 |----+----| H2 |
       | K1 |         | K2 |
       +----+         +----+           
          | \            
          | R/W         
          |   \      
          |     +----+
         R/O    | M1 |
          |     | S1 |
          |     +----+
          |       |
          |       |
       +----+     |   +----+
       | R2 |----+----| R3 |
       | S2 |         | S3 |
       +----+         +----+
