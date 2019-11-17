# MUD-HttpsLocal
Concerns on use of MUD for [HttpsLocal](https://httpslocal.github.io/proposals/#web-pki-approaches)'s DNS registration.

# Use case
**Situation:**
To access devices(e.g. cameras) at local network by a browser (e.g. smartphone) without rendezvous point at Internet.

**target:**
Secure communication without modifying Trust-store of browser. 

**Requirement:**
Certificates for local devices need to be issued from public trusted CA .

**Approach:**
Those devices will have local address, and FQDN under device vender’s Domain namespace.(So that, we do not need global Ip for public trusted EE certificate)

# Use of MUD for above use case
The method of importing certificates into devices, is out of scope here

![usecase-mud-httpslocal](https://user-images.githubusercontent.com/57828275/68990814-fb296000-089a-11ea-88cc-bbb8d9f6bf12.png)



# Issue
To handle nvr.example.com, local DNS server need to be delegated from nvr.example.com

Is it scope of MUD?

# Appendix
## Modified MUD File Example
This example contains a new field which is below.
- "thing-dnsname" : This is the domain name which is a portion of Thing's FQDN.

```json
{
  "ietf-mud:mud": {
    "mud-version": 1,
    "mud-url": "https://mud.example.com/camera-mud-httpslocal",
    "last-update": "2019-11-16T10:10:10+02:00",
    "cache-validity": 48,
    "is-supported": true,
    "systeminfo": "MUD-enabled Network Camera which can be accessed by browser",
    "thing-dnsname": "nvr.example.com",
    "from-device-policy": {
      "access-lists": {
        "access-list": [
          {
            "name": "mud-22111-v4fr"
          }
        ]
      }
    },
    "to-device-policy": {
      "access-lists": {
        "access-list": [
          {
            "name": "mud-22111-v4to"
          }
        ]
      }
    }
  },
  "ietf-access-control-list:acls": {
    "acl": [
      {
        "name": "mud-22111-v4to",
        "type": "ipv4-acl-type",
        "aces": {
          "ace": [
            {
              "name": "cl0-todev",
              "matches": {
                "ipv4": {
                  "ietf-acldns:src-dnsname": "fw-update.example.com",
                  "protocol": 6
                },
                "tcp": {
                  "ietf-mud:direction-initiated": "from-device",
                  "source-port": {
                    "operator": "eq",
                    "port": 443
                  }
                }
              },
              "actions": {
                "forwarding": "accept"
              }
            },
            {
              "name": "myctl0-todev",
              "matches": {
                "ietf-mud:mud": {
                  "my-controller": [
                    null
                  ]
                },
                "ipv4": {
                  "protocol": 6
                },
                "udp": {
                  "source-port": {
                    "operator": "eq",
                    "port": 443
                  }
                }
              },
              "actions": {
                "forwarding": "accept"
              }
            }
          ]
        }
      },
      {
        "name": "mud-22111-v4fr",
        "type": "ipv4-acl-type",
        "aces": {
          "ace": [
            {
              "name": "cl0-frdev",
              "matches": {
                "ipv4": {
                  "ietf-acldns:dst-dnsname": "fw-update.example.com",
                  "protocol": 6
                },
                "tcp": {
                  "ietf-mud:direction-initiated": "from-device",
                  "destination-port": {
                    "operator": "eq",
                    "port": 443
                  }
                }
              },
              "actions": {
                "forwarding": "accept"
              }
            },
            {
              "name": "myctl0-frdev",
              "matches": {
                "ietf-mud:mud": {
                  "my-controller": [
                    null
                  ]
                },
                "ipv4": {
                  "protocol": 6
                },
                "udp": {
                  "destination-port": {
                    "operator": "eq",
                    "port": 443
                  }
                }
              },
              "actions": {
                "forwarding": "accept"
              }
            }
          ]
        }
      }
    ]
  }
}
```
