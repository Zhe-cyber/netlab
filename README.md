# Netlab

## Week 6

The [Week 6 assignment page](https://zhe-cyber.github.io/netlab/week6/) describes the VXLAN overlay. From the repository root in the course container, run `python3 build_overlay.py overlay.yaml` to build it, or `python3 make_page6.py overlay.yaml` to regenerate the page.

### Lab wrap-up

1. A Mininet host is a process in its own Linux network namespace. A cable is a veth pair with one end inside the host namespace and the other plugged into the Linux bridge.
2. The underlay MTU was 1500 and its receiver throughput was 21.8 Gbit/s. The IPv4 VXLAN overlay MTU was 1450 and its receiver throughput was 11.1 Gbit/s (about 51% of the underlay). The 50 bytes are outer IPv4 (20), UDP (8), VXLAN (8) and the encapsulated Ethernet header (14). An outer IPv6 header is 40 bytes, raising overhead to 70 and lowering the VXLAN MTU to 1430.
3. Small pings still fit when the receiving underlay MTU dropped to 1400. Payload 1326 passed and 1327 failed. Both drop counters rose from 26 to 27. TCP connected but retransmitted because its larger segments were dropped. Once the MTU returned to 1500, large pings succeeded and the TCP receiver measured 12.3 Gbit/s.
4. A Docker container has its own filesystem and PID namespace in addition to its network namespace. The example showed Alpine 3.20.10, `sleep 600` at PID 1, and `eth0@if11` as the container end of a veth pair.
5. IPv4 VXLAN adds 50 bytes to the tenant IP packet and IPv6 VXLAN adds 70. The tunnel endpoints handle VXLAN and the intermediate switch forwards ordinary underlay Ethernet frames. Outer IP TTL or Hop Limit bounds routed loops. The Week 5 starter tunnel adds a four-byte header and each P4 switch on the tunnel path must understand it. The starter leaves the inner IPv4 TTL untouched, so the Week 5 assignment adds a tunnel hop limit that the P4 forwarding path decrements and uses to drop expired packets. The revised header size depends on the chosen width of that field.

## AI use

I used OpenAI Codex to help design the Week 6 overlay, implement the VTEP commands and MTU check, and double check the captured evidence.
