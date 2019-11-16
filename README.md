# MUD-HttpsLocal
Concerns on use of MUD for [HttpsLocal](https://httpslocal.github.io/proposals/#web-pki-approaches)'s DNS registoration.

# Use case
## Situation
To access devices(e.g. cameras) at local network by a browser (e.g. smartphone) without rendezvous point at Internet.

## target
Secure communication without modifying Trust-store of browser. 

## Requirement
Certificates for local devices need to be issued from public trusted CA .

## Approach
Those devices will have local address, and FQDN under device vender’s Domain namespace.(So that, we do not need global Ip for public trusted EE certificate)

# Use of MUD for above use case
The method of importing certificates into devices, is out of scope here

![usecase-mud-httpslocal](https://user-images.githubusercontent.com/57828275/68990814-fb296000-089a-11ea-88cc-bbb8d9f6bf12.png)



# Issue
To handle nvr.example.com, local DNS server need to be delegated from nvr.example.com

