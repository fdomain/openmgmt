---
layout: default
title: "gNMIC examples"
date: 2021-03-18 12:17:00 --0600
categories:
---

## overview

The following examples are used to find openconfig paths within Arista EOS using
the [gNMIC binary](https://gnmic.kmrd.dev/). There will be some commands which
will specify one liners which will have the address of the gNMI target and some
commands which will use the `.gnmic.yaml` file which will have the target
information inside declared. All outputs will be redirected to the outputs file
with the .json extension so they are easier to view within a text editor.

## Download && install gnmic

To install run:

```shell
curl -sL https://github.com/karimra/gnmic/raw/master/install.sh | sudo bash
```

To get the version run:

```shell
gnmi_stuff$ gnmic version
```

Output:

```shell
version : 0.17.0
 commit : 278661e
   date : 2021-07-14T07:29:14Z
 gitURL : https://github.com/karimra/gnmic
   docs : https://gnmic.kmrd.dev
```

## Device config

```shell

management api gnmi
   transport grpc default
   provider eos-native

ceos3#                show management api gnmi
Octa:               enabled
Enabled:            Yes
Server:             running on port 6030
SSL Profile:        none
QoS DSCP:           none
Authorization Required:No
```

```shell
gnmic -a 127.0.0.1:6030 -u admin -p admin --insecure capabilities  \
  >> outputs/capabilities.json
```

## gNMI GET RPC Examples

### OpenConfig paths

#### Get all information

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip get --path "/"
```

#### Get the BGP configuration in the default VRF

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip get --path  \
  '/network-instances/network-instance[name=default]/protocols/protocol[identifier=BGP][name=BGP]/bgp'`
```

#### Get BGP neighbors

```shell
gnmi -addr 10.83.13.139 -username admin get \
  '/network-instances/network-instance[name=default]/protocols/protocol[name=BGP]/bgp/neighbors'
```

#### Get all interface descriptions

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip get --path  \
  '/interfaces/interface/subinterfaces/subinterface/state/description'
```

#### Get an interface's description

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip get --path  \
  'interfaces/interface[name=Ethernet1]/subinterfaces/subinterface/state/description'
```

```javascript
[
  {
    "timestamp": 1626293671204274106,
    "time": "2021-07-14T21:14:31.204274106+01:00",
    "updates": [
      {
        "Path": "interfaces/interface[name=Ethernet1]/subinterfaces/subinterface[index=0]/state/description",
        "values": {
          "interfaces/interface/subinterfaces/subinterface/state/description": "To-SPINE1"
        }
      }
    ]
  }
]
```

#### Get the operational status of all interfaces

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip get --path  \
  'interfaces/interface/state/oper-status'
```

<details><summary> Reveal output</summary>
<p>

```javascript
[
  {
    "timestamp": 1626293641102904713,
    "time": "2021-07-14T21:14:01.102904713+01:00",
    "updates": [
      {
        "Path": "interfaces/interface[name=Management1]/state/oper-status",
        "values": {
          "interfaces/interface/state/oper-status": "UP"
        }
      },
      {
        "Path": "interfaces/interface[name=Ethernet1]/state/oper-status",
        "values": {
          "interfaces/interface/state/oper-status": "UP"
        }
      },
      {
        "Path": "interfaces/interface[name=Ethernet5]/state/oper-status",
        "values": {
          "interfaces/interface/state/oper-status": "UP"
        }
      },
      {
        "Path": "interfaces/interface[name=Ethernet4]/state/oper-status",
        "values": {
          "interfaces/interface/state/oper-status": "UP"
        }
      },
      {
        "Path": "interfaces/interface[name=Ethernet3]/state/oper-status",
        "values": {
          "interfaces/interface/state/oper-status": "UP"
        }
      },
      {
        "Path": "interfaces/interface[name=Ethernet2]/state/oper-status",
        "values": {
          "interfaces/interface/state/oper-status": "UP"
        }
      },
      {
        "Path": "interfaces/interface[name=Port-Channel4]/state/oper-status",
        "values": {
          "interfaces/interface/state/oper-status": "UP"
        }
      },
      {
        "Path": "interfaces/interface[name=Port-Channel3]/state/oper-status",
        "values": {
          "interfaces/interface/state/oper-status": "UP"
        }
      }
    ]
  }
]
```

</p>
</details>

#### Get all states of an interface

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip get --path  \
  'interfaces/interface[name=Ethernet1]/state/'
```

<details><summary> Reveal output</summary>
<p>

```javascript
[[
  {
    "timestamp": 1626293429376469348,
    "time": "2021-07-14T21:10:29.376469348+01:00",
    "updates": [
      {
        "Path": "interfaces/interface[name=Ethernet1]/state",
        "values": {
          "interfaces/interface/state": {
            "arista-intf-augments:inactive": false,
            "openconfig-interfaces:admin-status": "UP",
            "openconfig-interfaces:counters": {
              "in-broadcast-pkts": "0",
              "in-discards": "0",
              "in-errors": "0",
              "in-fcs-errors": "0",
              "in-multicast-pkts": "48",
              "in-octets": "111398",
              "in-unicast-pkts": "1460",
              "out-broadcast-pkts": "1",
              "out-discards": "0",
              "out-errors": "0",
              "out-multicast-pkts": "49",
              "out-octets": "117389",
              "out-unicast-pkts": "1457"
            },
            "openconfig-interfaces:description": "To-SPINE1",
            "openconfig-interfaces:ifindex": 1,
            "openconfig-interfaces:last-change": "1626292006733589760",
            "openconfig-interfaces:mtu": 0,
            "openconfig-interfaces:name": "Ethernet1",
            "openconfig-interfaces:oper-status": "UP",
            "openconfig-interfaces:type": "iana-if-type:ethernetCsmacd",
            "openconfig-platform-port:hardware-port": "Port1"
          }
        }
      }
    ]
  }
]
```

</p>
</details>

#### Get an interface's operational status

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip get --path  \
  'interfaces/interface[name=Ethernet24]/state/oper-status'
```

```text
[
  {
    "timestamp": 1626293414057239130,
    "time": "2021-07-14T21:10:14.05723913+01:00",
    "updates": [
      {
        "Path": "interfaces/interface[name=Ethernet2]/state/oper-status",
        "values": {
          "interfaces/interface/state/oper-status": "UP"
        }
      }
    ]
  }
]
```

#### Get an interface's admin status

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip get --path  \
  'interfaces/interface[name=Ethernet1]/state/admin-status'
```

```text
[
  {
    "timestamp": 1626293085257457373,
    "time": "2021-07-14T21:04:45.257457373+01:00",
    "updates": [
      {
        "Path": "interfaces/interface[name=Ethernet1]/state/admin-status",
        "values": {
          "interfaces/interface/state/admin-status": "UP"
        }
      }
    ]
  }
]
```

#### Get the DOM metrics of all interfaces

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip get --path  \
  "components/component/transceiver/physical-channels/channel/state/"
```

<details><summary> Reveal output</summary>
<p>

```javascript
[
  {
    "timestamp": 1626293085257457373,
    "time": "2021-07-14T21:04:45.257457373+01:00",
    "updates": [
      {
        "Path": "components/component[name=Ethernet49 transceiver]/transceiver/physical-channels/channel[index=0]/state",
        "values": {
          "components/component/transceiver/physical-channels/channel/state": {
            "openconfig-platform-transceiver:index": 0,
            "openconfig-platform-transceiver:input-power": {
              "instant": "1.15"
            },
            "openconfig-platform-transceiver:laser-bias-current": {
              "instant": "0.0"
            },
            "openconfig-platform-transceiver:output-power": {}
          }
        }
      },
      {
        "Path": "components/component[name=Ethernet50 transceiver]/transceiver/physical-channels/channel[index=0]/state",
        "values": {
          "components/component/transceiver/physical-channels/channel/state": {
            "openconfig-platform-transceiver:index": 0,
            "openconfig-platform-transceiver:input-power": {
              "instant": "1.11"
            },
            "openconfig-platform-transceiver:laser-bias-current": {
              "instant": "0.0"
            },
            "openconfig-platform-transceiver:output-power": {}
          }
        }
      }
    ]
  }
]
```

</p>
</details>

#### Get the DOM metrics of an interface

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip get --path  \
  "components/component[name=Ethernet49 transceiver]/transceiver/physical-channels/channel/state/"
```

<details><summary> Reveal output</summary>
<p>

```javascript
[
  {
    "time": "1970-01-01T01:00:00+01:00",
    "updates": [
      {
        "Path": "components/component[name=Ethernet49 transceiver]/transceiver/physical-channels/channel[index=0]/state",
        "values": {
          "components/component/transceiver/physical-channels/channel/state": {
            "openconfig-platform-transceiver:index": 0,
            "openconfig-platform-transceiver:input-power": {
              "instant": "1.08"
            },
            "openconfig-platform-transceiver:laser-bias-current": {
              "instant": "0.0"
            },
            "openconfig-platform-transceiver:output-power": {}
          }
        }
      }
    ]
  }
]

```

</p>
</details>

#### Get per core CPU utilization

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip get --path  \
  'components/component/cpu'
```

<details><summary> Reveal output</summary>
<p>

```javascript
[
  {
    "timestamp": 1626294810779644595,
    "time": "2021-07-14T21:33:30.779644595+01:00",
    "updates": [
      {
        "Path": "components/component[name=CPU1]/cpu",
        "values": {
          "components/component/cpu": {
            "openconfig-platform-cpu:utilization": {
              "state": {
                "avg": 6,
                "instant": 6,
                "interval": "1000000000000",
                "max": 14,
                "max-time": "3252585270429488128",
                "min": 2,
                "min-time": "3252586330424618496"
              }
            }
          }
        }
      },
      {
        "Path": "components/component[name=CPU0]/cpu",
        "values": {
          "components/component/cpu": {
            "openconfig-platform-cpu:utilization": {
              "state": {
                "avg": 5,
                "instant": 7,
                "interval": "1000000000000",
                "max": 13,
                "max-time": "3252586170423993344",
                "min": 1,
                "min-time": "3252585340433252352"
              }
            }
          }
        }
      }
    ]
  }
]

```

</p>
</details>

#### Get the available/utilized memory

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip get --path  \
  'components/component/state/memory/'
```

```javascript
[
  {
    "timestamp": 1626294846423115892,
    "time": "2021-07-14T21:34:06.423115892+01:00",
    "updates": [
      {
        "Path": "components/component[name=Chassis]/state/memory",
        "values": {
          "components/component/state/memory": {
            "openconfig-platform:available": "4098412544",
            "openconfig-platform:utilized": "4018016256"
          }
        }
      }
    ]
  }
]
```

#### Get the system environment temperatures

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip get --path  \
   'components/component/state/temperature/instant'
```

<details><summary> Reveal output</summary>
<p>

```javascript
[
  {
    "timestamp": 1626294876186613578,
    "time": "2021-07-14T21:34:36.186613578+01:00"
    "updates": [
      {
        "Path": "components/component[name=DomTemperatureSensor50]/state/temperature/instant",
        "values": {
          "components/component/state/temperature/instant": {
            "digits": 2709375,
            "precision": 5
          }
        }
      },
      {
        "Path": "components/component[name=DomTemperatureSensor49]/state/temperature/instant",
        "values": {
          "components/component/state/temperature/instant": {
            "digits": 27140625,
            "precision": 6
          }
        }
      },
      {
        "Path": "components/component[name=TempSensor7]/state/temperature/instant",
        "values": {
          "components/component/state/temperature/instant": {
            "digits": 41168000000000006,
            "precision": 15
          }
        }
      },
      {
        "Path": "components/component[name=TempSensor8]/state/temperature/instant",
        "values": {
          "components/component/state/temperature/instant": {
            "digits": 4225279999999998,
            "precision": 14
          }
        }
      },
      {
        "Path": "components/component[name=TempSensor9]/state/temperature/instant",
        "values": {
          "components/component/state/temperature/instant": {
            "digits": 4062560000000002,
            "precision": 14
          }
        }
      },
      {
        "Path": "components/component[name=TempSensor1]/state/temperature/instant",
        "values": {
          "components/component/state/temperature/instant": {
            "digits": 39141723894742796,
            "precision": 15
          }
        }
      },
      {
        "Path": "components/component[name=TempSensor6]/state/temperature/instant",
        "values": {
          "components/component/state/temperature/instant": {
            "digits": 23875,
            "precision": 3
          }
        }
      },
      {
        "Path": "components/component[name=TempSensor3]/state/temperature/instant",
        "values": {
          "components/component/state/temperature/instant": {
            "digits": 21
          }
        }
      },
      {
        "Path": "components/component[name=TempSensor4]/state/temperature/instant",
        "values": {
          "components/component/state/temperature/instant": {
            "digits": 1725,
            "precision": 2
          }
        }
      },
      {
        "Path": "components/component[name=TempSensor5]/state/temperature/instant",
        "values": {
          "components/component/state/temperature/instant": {
            "digits": 29
          }
        }
      },
      {
        "Path": "components/component[name=TempSensor2]/state/temperature/instant",
        "values": {
          "components/component/state/temperature/instant": {
            "digits": 32
          }
        }
      }
    ]
  }
]

```

</p>
</details>

## gNMI SUBSCRIBE RPC Examples

### OpenConfig paths

#### Subscribe to all BGP neighbor states

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip subscribe --path  \
  '/network-instances/network-instance/protocols/protocol/bgp/neighbors/neighbor/state'
```

<details><summary> Reveal output</summary>
<p>

```javascript
{
  "source": "10.83.13.214:6030",
  "subscription-name": "default-1626302647",
  "timestamp": 1626292010055258009,
  "time": "2021-07-14T20:46:50.055258009+01:00",
  "updates": [
    {
      "Path": "network-instances/network-instance[name=default]/protocols/protocol[identifier=BGP][name=BGP]/bgp/neighbors/neighbor[neighbor-address=172.18.255.42]/state/messages/sent/UPDATE",
      "values": {
        "network-instances/network-instance/protocols/protocol/bgp/neighbors/neighbor/state/messages/sent/UPDATE": 8
      }
    }
  ]
}
{
  "source": "10.83.13.214:6030",
  "subscription-name": "default-1626302647",
  "timestamp": 1626292008053994815,
  "time": "2021-07-14T20:46:48.053994815+01:00",
  "updates": [
    {
      "Path": "network-instances/network-instance[name=default]/protocols/protocol[identifier=BGP][name=BGP]/bgp/neighbors/neighbor[neighbor-address=172.18.255.42]/state/established-transitions",
      "values": {
        "network-instances/network-instance/protocols/protocol/bgp/neighbors/neighbor/state/established-transitions": 1
      }
    }
  ]
}
```

</p>
</details>

#### Subscribe to specific BGP neighbor state

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip subscribe --path  \
  'network-instances/network-instance[name=Tenant_A_WEB_Zone]/protocols/protocol[identifier=BGP][name=BGP]/bgp/neighbors/neighbor[neighbor-address=10.255.251.21]/state'
```

<details><summary> Reveal output</summary>
<p>

```javascript
{
  "source": "10.83.13.214:6030",
  "subscription-name": "default-1626303017",
  "timestamp": 1626302991598465536,
  "time": "2021-07-14T23:49:51.598465536+01:00",
  "updates": [
    {
      "Path": "network-instances/network-instance[name=Tenant_A_WEB_Zone]/protocols/protocol[identifier=BGP][name=BGP]/bgp/neighbors/neighbor[neighbor-address=10.255.251.21]/state/last-established",
      "values": {
        "network-instances/network-instance/protocols/protocol/bgp/neighbors/neighbor/state/last-established": 1626302991563603200
      }
    }
  ]
}
```

</p>
</details>

#### Subscribe with stream mode sample and interval

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip --sample-interval 5s --stream-mode sample subscribe --path  \
  '/network-instances/network-instance[name=Tenant_A_WEB_Zone]/protocols/protocol[identifier=BGP][name=BGP]/bgp/neighbors/neighbor[neighbor-address=10.255.251.21]/afi-safis/afi-safi[afi-safi-name=openconfig-bgp-types:IPV4_UNICAST]/state/prefixes/received'
```

<details><summary> Reveal output</summary>
<p>

```javascript
{
  "source": "10.83.13.214:6030",
  "subscription-name": "default-1626303393",
  "timestamp": 1626303158135414182,
  "time": "2021-07-14T23:52:38.135414182+01:00",
  "updates": [
    {
      "Path": "network-instances/network-instance[name=Tenant_A_WEB_Zone]/protocols/protocol[identifier=BGP][name=BGP]/bgp/neighbors/neighbor[neighbor-address=10.255.251.21]/afi-safis/afi-safi[afi-safi-name=IPV4_UNICAST]/state/prefixes/received",
      "values": {
        "network-instances/network-instance/protocols/protocol/bgp/neighbors/neighbor/afi-safis/afi-safi/state/prefixes/received": 5
      }
    }
  ]
}
{
  "source": "10.83.13.214:6030",
  "subscription-name": "default-1626303393",
  "timestamp": 1626303158135414182,
  "time": "2021-07-14T23:52:38.135414182+01:00",
  "updates": [
    {
      "Path": "network-instances/network-instance[name=Tenant_A_WEB_Zone]/protocols/protocol[identifier=BGP][name=BGP]/bgp/neighbors/neighbor[neighbor-address=10.255.251.21]/afi-safis/afi-safi[afi-safi-name=IPV4_UNICAST]/state/prefixes/received",
      "values": {
        "network-instances/network-instance/protocols/protocol/bgp/neighbors/neighbor/afi-safis/afi-safi/state/prefixes/received": 5
      }
    }
  ]
}
```

</p>
</details>

#### Subscribe to interface counters and save them to a file

```shell
gnmic -a 127.0.0.1:6030 -u admin -p admin --insecure subscribe --path \
  "/interfaces/interface/state/counters"  >> outputs/interface_state.json
```

## gNMI SET RPC Examples

### OpenConfig paths

#### Configure BGP neighbor address and peer AS

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip set \
--update-path '/network-instances/network-instance[name=default]/protocols/protocol[name=BGP]/bgp/neighbors/neighbor[neighbor-address=10.10.100.43]' \
--update-file value.json
```

**value.json**:

```javascript
{"config": {"neighbor-address":"10.10.100.43", "peer-as": 123}}
```

Output:

```javascript
{
  "timestamp": 1626305876151537555,
  "time": "2021-07-15T00:37:56.151537555+01:00",
  "results": [
    {
      "operation": "UPDATE",
      "path": "network-instances/network-instance[name=default]/protocols/protocol[name=BGP]/bgp/neighbors/neighbor[neighbor-address=10.10.100.43]"
    }
  ]
}
```

#### Configure BGP neighbor address, peer AS and send-community

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip set  \
--update-path '/network-instances/network-instance[name=default]/protocols/protocol[name=BGP]/bgp/neighbors/neighbor[neighbor-address=10.10.100.43]' \
--update-file value.json
```

**value.json**:

```javascript
{"config": {"neighbor-address":"10.10.100.43", "peer-as": 123, "enabled": true, "send-community": "EXTENDED"}}
```

Output:

```javascript
{
  "timestamp": 1626305480401353997,
  "time": "2021-07-15T00:31:20.401353997+01:00",
  "results": [
    {
      "operation": "UPDATE",
      "path": "network-instances/network-instance[name=default]/protocols/protocol[name=BGP]/bgp/neighbors/neighbor[neighbor-address=10.10.100.43]"
    }
  ]
}
```

#### Create BGP peer group

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip set \
  --update-path '/network-instances/network-instance[name=default]/protocols/protocol[name=BGP]/bgp/peer-groups/peer-group[peer-group-name=XYZ]' \
  --update-file value.json
```

**value.json**:

```javascript
{"config": {"peer-group-name":"XYZ", "local-as": 114}}
```

#### Update BGP peer AS

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip set \
--update-path '/network-instances/network-instance[name=default]/protocols/protocol[name=BGP]/bgp/neighbors/neighbor[neighbor-address=10.10.100.43]/config/peer-as' \
--update-value '110'
```

Output:

```javascript
{
  "timestamp": 1626306067189329813,
  "time": "2021-07-15T00:41:07.189329813+01:00",
  "results": [
    {
      "operation": "UPDATE",
      "path": "network-instances/network-instance[name=default]/protocols/protocol[name=BGP]/bgp/neighbors/neighbor[neighbor-address=10.10.100.43]/config/peer-as"
    }
  ]
}
```

#### Update BGP peer group

```shell
gnmic -a 10.83.13.214:6030 -u cvpadmin -p arastra --insecure --gzip set \
--update-path '/network-instances/network-instance[name=default]/protocols/protocol[name=BGP]/bgp/neighbors/neighbor[neighbor-address=10.10.100.43]/config/peer-group' \
--update-value 'XYZ'
```

Output:

```javascript
{
  "timestamp": 1626306067189329813,
  "time": "2021-07-15T00:41:07.189329813+01:00",
  "results": [
    {
      "operation": "UPDATE",
      "path": "network-instances/network-instance[name=default]/protocols/protocol[name=BGP]/bgp/neighbors/neighbor[neighbor-address=10.10.100.43]/config/peer-as"
    }
  ]
}
```

#### Create an ACL

```shell

gnmic -a 10.83.13.108:6030 -u cvpadmin -p arastra --insecure --gzip set \
--update-path /acl/acl-sets \
--update-file acl2.json
```

Output:

```javascript
{
  "timestamp": 1626307972085688242,
  "time": "2021-07-15T01:12:52.085688242+01:00",
  "results": [
    {
      "operation": "UPDATE",
      "path": "acl/acl-sets"
    }
  ]
}
```

<details><summary> acl2.json </summary>
<p>

```javascript
{
   "acl-set": [
      {
         "config": {
            "name": "test",
            "type": "ACL_IPV4"
         },
         "name": "test",
         "type": "ACL_IPV4",
         "acl-entries": {
            "acl-entry": [
               {
                  "sequence-id": 10,
                  "actions": {
                     "config": {
                        "forwarding-action": "DROP"
                     }
                  },
                  "config": {
                     "sequence-id": 10
                  },
                  "ipv4": {
                     "config": {
                        "destination-address": "1.2.3.4/12",
                        "source-address": "0.0.0.0/0"
                     }
                  }
               }
            ]
         }
      }
   ]
}
```

</p>
</details>

This creates

```text
ip access-list test
   10 deny ip any 1.0.0.0/12
```

#### Shutdown an interface

```shell
gnmic -a 10.83.13.108:6030 -u cvpadmin -p arastra --insecure --gzip set \
--update-path '/interfaces/interface[name=Ethernet1]/config/enabled' \
--update-value 'false'
```

```javascript
{
  "timestamp": 1626309145489047571,
  "time": "2021-07-15T01:32:25.489047571+01:00",
  "results": [
    {
      "operation": "UPDATE",
      "path": "interfaces/interface[name=Ethernet1]/config/enabled"
    }
  ]
}
```

#### Bring up an interface

```shell
gnmic -a 10.83.13.108:6030 -u cvpadmin -p arastra --insecure --gzip set \
--update-path '/interfaces/interface[name=Ethernet1]/config/enabled' \
--update-value 'true'
```

```javascript
{
  "timestamp": 1626309156607307596,
  "time": "2021-07-15T01:32:36.607307596+01:00",
  "results": [
    {
      "operation": "UPDATE",
      "path": "interfaces/interface[name=Ethernet1]/config/enabled"
    }
  ]
}
```

## EOS Native paths

To get EOS native paths, OCTA has to be enabled as mentioned in the
configuration section. Performing GET/SUBSCRIBE actions using EOS native paths
require changing the origin to `eos_native`.

### Commonly used paths

- MAC table: `/Smash/bridging/status/smashFdbStatus`
- ARP table: `/Smash/arp/status/arpEntry`
- Neighbor table: `/Smash/arp/status/neighborEntry`
- IPv4 RIB: `/Smash/routing/status/route`
- IPv6 RIB: `/Smash/routing6/status/route`
- IPv4 next-hop table: `/Smash/routing/status/nexthop`
- IPv6 next-hop table: `/Smash/routing6/status/nexthop`
- CPU info: `/Kernel/proc/cpu`
- Process statistics: `/Kernel/proc/stat`
- System info: `/Kernel/sysinfo`
- EOS version: `/Eos/image`
- Interface counters: `/Smash/counters/ethIntf/<agent>/current/counter`
  - Values for `<agent>` are:
    - 7500-family, 7280-family, 7020-family (Arad/Jericho ASICs): `SandCounters`
    - 7300-family, 7250-family, 7050-family, 7010 products, 720-family (Trident
      ASICs): `StrataCounters`
    - For 7060-family, 7260-family (Tomahawk): `Strata-FixedSystem` or
      `StrataCounters` from 4.22+
    - 7150-family products (Alta ASICs): `FocalPointV2`
    - 7160-family products (Cavium/Xpliant ASICs): `XpCounters`
    - 7170-family products (Barefoot ASIC): `BfnCounters`

### Get CPU utilization

```shell
 gnmic -a 10.83.13.108:6030 -u cvpadmin -p arastra --insecure --gzip get --path  \
  'eos_native:/Kernel/proc/cpu/utilization/total'
```

<details><summary> Reveal output</summary>
<p>

```javascript
[
  {
    "timestamp": 1626291721294738334,
    "time": "2021-07-14T20:42:01.294738334+01:00",
    "prefix": "eos_native:Kernel/proc/cpu/utilization/total",
    "updates": [
      {
        "Path": "name",
        "values": {
          "name": "total"
        }
      },
      {
        "Path": "nice",
        "values": {
          "nice": 28845
        }
      },
      {
        "Path": "util",
        "values": {
          "util": 41
        }
      },
      {
        "Path": "user",
        "values": {
          "user": 32058347
        }
      },
      {
        "Path": "system",
        "values": {
          "system": 4597389
        }
      },
      {
        "Path": "idle",
        "values": {
          "idle": 76373566
        }
      }
    ]
  }
]
```

</p>
</details>

### Get transceiver DOM temperature

```shell
gnmic -a 127.0.0.1:6030 -u admin -p admin --insecure --gzip get --path \
  eos_native:/Sysdb/hardware/archer/xcvr/status >> outputs/doms.json
```

### Get EOS image version

```shell
gnmic -a 127.0.0.1:6030 -u admin -p admin --insecure --gzip get \
  --path eos_native:/Eos/image >> outputs/eos_image.json
```

## Cli origin

### Get the running config

```shell

gnmic -a 127.0.0.1:6030 -u admin -p admin --insecure  get \
  --path "cli:/show running-config" >> outputs/outputs.json
```

### Get the total route count

```shell
gnmic -a 127.0.0.1:6030 -u admin -p admin --insecure  get \
  --path "cli:/show ip route summary" \
  | jq '.[0].updates[0].values."show ip route summary".totalRoutes'
```

## Misc

### Save all status states to a file

```shell
gnmic -a 127.0.0.1:6030 -u admin -p admin --insecure get \
  --path ".../state/..." >> outputs/states.json
```

### Save all config states to a file

```shell
gnmic -a 127.0.0.1:6030 -u admin -p admin --insecure get \
  --path ".../config/..." >> outputs/configs.json
```

### Save network instance states to a file

```shell
gnmic -a 127.0.0.1:6030 -u admin -p admin --insecure get --path \
  /network-instances/network-instance >> outputs/network-instances.json
```

### Save BGP states to a file

```shell
gnmic -a 127.0.0.1:6030 -u admin -p admin --insecure get --path \
  /network-instances/network-instance[name=default]/protocols/protocol[name=BGP]\
  >> outputs/bgp.json
```