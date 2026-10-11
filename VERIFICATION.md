# ✅ Verification & Test Results

## 1. HSRP — HQ Active/Standby (real output from this lab)

```
HQ-R1# show standby brief

Interface   Grp  Prio P State    Active          Standby         Virtual IP
Gi0/1       1    110  P Active   local           192.168.199.2   192.168.199.254
```

```
HQ-R2# show standby brief

Interface   Grp  Prio P State    Active          Standby         Virtual IP
Gi0/1       1    100  P Standby  192.168.199.1   local           192.168.199.254
```

**Failover test:** shut HQ-R1's tracked uplink `Gi0/0` → priority drops 110→90 →
HQ-R2 takes Active on `192.168.199.254` → LAN clients survive with 1–3 lost pings.

## 2. DMVPN Tunnel (Hub ↔ Spokes)

Per CONFIGURATION.md: mGRE Tunnel0, NHRP network-id 100, key `DMVPN-KEY`,
IPsec profile `DMVPN-PROFILE`, hub `10.100.0.1`, BR1 `10.100.0.2`.

```
HQ-R1# show dmvpn
 ->  Expected: Status OK, attribute: NBMA 203.0.113.1, tunnel 10.100.0.2 (BR1) registered via NHRP
HQ-R1# show ip nhrp
 ->  Expected: dynamic registration entry for 10.100.0.2
```

## 3. EIGRP over DMVPN (AS 100)

```
HQ-R1# show ip eigrp neighbors
 ->  Expected: neighbor 10.100.0.2 (BR1) via Tunnel0, AS 100
HQ-R1# show ip route eigrp
 ->  Expected: 192.168.110.0/24 (BR1 LAN) learned via Tunnel0 (D)
```

## 4. End-to-End Connectivity (through the tunnel)

```
HQ-LAN-PC(192.168.199.x)# ping 192.168.110.10
 ->  Expected: 100% success — traffic rides the encrypted DMVPN tunnel (verify with
     'show crypto ipsec sa' packet counters increasing during the ping)
```

---
*Reproduce with the topology screenshot in README.md + CONFIGURATION.md.*
