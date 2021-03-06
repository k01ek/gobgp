# Configuration Example

```yaml
bmp-servers:
- config:
    address: 127.0.0.1
    port: 11019
    route-monitoring-policy: pre-policy
    statistics-timeout: 3600
defined-sets:
  bgp-defined-sets:
    as-path-sets:
    - as-path-list:
      - ^100
      - 200$
      as-path-set-name: as0
    community-sets:
    - community-list:
      - 100:100
      community-set-name: cs0
    ext-community-sets:
    - ext-community-list:
      - rt:100:100
      - soo:200:200
      ext-community-set-name: es0
    large-community-sets:
    - large-community-list:
      - 100:100:100
      - 200:200:200
      large-community-set-name: ls0
  neighbor-sets:
  - neighbor-info-list:
    - 192.168.10.2
    - 172.13.0.0/24
    neighbor-set-name: ns0
  prefix-sets:
  - prefix-list:
    - ip-prefix: 10.0.0.0/8
      masklength-range: 24..32
    prefix-set-name: ps0
dynamic-neighbors:
- config:
    peer-group: my-peer-group
    prefix: 20.0.0.0/24
global:
  apply-policy:
    config:
      default-export-policy: accept-route
      default-import-policy: reject-route
      export-policy-list:
      - policy2
      import-policy-list:
      - policy1
  config:
    as: 1
    local-address-list:
    - 192.168.10.1
    - 2001:db8::1
    port: 1790
    router-id: 1.1.1.1
mrt-dump:
- config:
    dump-interval: 180
    dump-type: updates
    file-name: /tmp/log/2006/01/02.1504.dump
neighbors:
- add-paths:
    config:
      receive: true
      send-max: 8
  afi-safis:
  - add-paths:
      config:
        receive: true
        send-max: 8
    config:
      afi-safi-name: ipv4-unicast
    long-lived-graceful-restart:
      config:
        enabled: true
        restart-time: 100000
    mp-graceful-restart:
      config:
        enabled: true
    prefix-limit:
      config:
        max-prefixes: 1000
        shutdown-threshold-pct: 80
  - config:
      afi-safi-name: ipv6-unicast
  - config:
      afi-safi-name: ipv4-labelled-unicast
  - config:
      afi-safi-name: ipv6-labelled-unicast
  - config:
      afi-safi-name: l3vpn-ipv4-unicast
  - config:
      afi-safi-name: l3vpn-ipv6-unicast
  - config:
      afi-safi-name: l2vpn-evpn
  - config:
      afi-safi-name: rtc
  - config:
      afi-safi-name: ipv4-encap
  - config:
      afi-safi-name: ipv6-encap
  - config:
      afi-safi-name: ipv4-flowspec
  - config:
      afi-safi-name: ipv6-flowspec
  - config:
      afi-safi-name: opaque
  apply-policy:
    config:
      default-export-policy: accept-route
      default-import-policy: reject-route
      default-in-policy: reject-route
      export-policy-list:
      - policy2
      import-policy-list:
      - policy1
      in-policy-list:
      - policy3
  as-path-options:
    config:
      allow-own-as: 1
      replace-peer-as: true
  config:
    auth-password: password
    local-as: 1000
    neighbor-address: 192.168.10.2
    peer-as: 2
    remove-private-as: all
  ebgp-multihop:
    config:
      enabled: true
      multihop-ttl: 100
  graceful-restart:
    config:
      enabled: true
      long-lived-enabled: true
      notification-enabled: true
      restart-time: 20
  route-reflector:
    config:
      route-reflector-client: true
      route-reflector-cluster-id: 192.168.0.1
  route-server:
    config:
      route-server-client: true
  timers:
    config:
      connect-retry: 5
      hold-time: 9
      keepalive-interval: 3
  transport:
    config:
      local-address: 192.168.10.1
      passive-mode: true
      remote-port: 2016
      ttl: 64
peer-groups:
- afi-safis:
  - config:
      afi-safi-name: ipv4-unicast
  config:
    peer-as: 65000
    peer-group-name: my-peer-group
policy-definitions:
- name: policy1
  statements:
  - actions:
      bgp-actions:
        set-as-path-prepend:
          as: last-as
          repeat-n: 5
      route-disposition: accept-route
    conditions:
      bgp-conditions:
        match-community-set:
          community-set: cs0
          match-set-options: all
        match-large-community-set:
          large-community-set: ls0
          match-set-options: all
      match-neighbor-set:
        match-set-options: invert
        neighbor-set: ns0
      match-prefix-set:
        match-set-options: any
        prefix-set: ps0
  - actions:
      route-disposition: reject-route
    conditions:
      bgp-conditions:
        match-ext-community-set:
          ext-community-set: es0
- name: policy2
  statements:
  - actions:
      bgp-actions:
        set-community:
          options: add
          set-community-method:
            communities-list:
            - 100:200
      route-disposition: accept-route
    conditions:
      bgp-conditions:
        match-as-path-set:
          as-path-set: as0
- name: policy3
  statements:
  - actions:
      bgp-actions:
        set-community:
          options: add
          set-community-method:
            communities-list:
            - 100:200
      route-disposition: accept-route
    conditions:
      bgp-conditions:
        match-as-path-set:
          as-path-set: as0
  - actions:
      bgp-actions:
        set-ext-community:
          options: replace
          set-ext-community-method:
            communities-list:
            - soo:100:200
            - rt:300:400
      route-disposition: accept-route
    conditions:
      match-prefix-set:
        match-set-options: invert
        prefix-set: ps0
  - actions:
      bgp-actions:
        set-ext-community:
          options: remove
          set-ext-community-method:
            communities-list:
            - soo:500:600
            - rt:700:800
      route-disposition: accept-route
    conditions:
      match-neighbor-set:
        neighbor-set: ns0
  - actions:
      route-disposition: accept-route
    conditions:
      bgp-conditions:
        next-hop-in-list:
        - 10.0.100.1/32
- name: route-type-policy
  statements:
  - actions:
      route-disposition: accept-route
    conditions:
      bgp-conditions:
        route-type: local
  - actions:
      route-disposition: accept-route
    conditions:
      bgp-conditions:
        route-type: internal
  - actions:
      route-disposition: accept-route
    conditions:
      bgp-conditions:
        route-type: external
- name: large-communty-policy
  statements:
  - actions:
      bgp-actions:
        set-large-community:
          options: add
          set-large-community-method:
            communities-list:
            - 100:200:300
      route-disposition: accept-route
  - actions:
      bgp-actions:
        set-large-community:
          options: replace
          set-large-community-method:
            communities-list:
            - 100:200:300
      route-disposition: accept-route
  - actions:
      bgp-actions:
        set-large-community:
          options: remove
          set-large-community-method:
            communities-list:
            - 100:200:300
            - '^200:'
      route-disposition: accept-route
rpki-servers:
- config:
    address: 210.173.170.254
    port: 323
vrfs:
- config:
    both-rt-list:
    - 65000:100
    id: 1
    name: vrf1
    rd: 65000:100
zebra:
  config:
    enabled: true
    redistribute-route-type-list:
    - connect
    url: unix:/var/run/quagga/zserv.api
    version: 2
```
