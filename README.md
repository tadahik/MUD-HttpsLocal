# MUD-HttpsLocal
Concerns on use of MUD for HttpsLocal's DNS

## Use case
- Situation
-- To access devices(e.g. cameras) at local network by a browser (e.g. smartphone) without rendezvous point at Internet.

- target
-- We want secure communication without modifying Trust-store of browser. 

- Requirement]
-- Certificates for local devices need to be issued from public trusted CA .

- solution
-- Those devices will have local address, and FQDN under device vender’s Domain namespace.(So that, we do not need global Ip for public trusted EE certificate)

## Use of MUD for above use case
-the method of importing certificates into devices is out of scope here
