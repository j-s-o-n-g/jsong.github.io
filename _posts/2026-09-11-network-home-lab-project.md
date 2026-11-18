---
title: "Network Home Lab Project with Packet Tracer"
date: 2026-09-11
categories: [Network]
tags: [network, homelab, packet tracer, cisco]     # TAG names should always be lowercase
comments: true
---

I am currently balancing a cloud project, certification study, my job search, and interviews. I need to get those commitments organized first. I will return to finish this Packet Tracer home lab post, including the configurations, troubleshooting process, verification results, and screenshots I create myself.

## Incidents to recreate in the lab

The following are **planned, fictional workplace scenarios** based on the networking skills I practiced. They are prompts for future Packet Tracer builds, not a record of completed incident resolutions.

### Switching and link aggregation

1. **Branch uplink after a switch change.** A branch loses access to two authorized VLANs after someone edits its uplink. Another inter-switch link reports a native VLAN mismatch, and a redundant pair of links is no longer operating as one. Inspect the trunk paths, restore the intended VLAN boundaries, and check that LACP forms a stable bundle without changing unrelated traffic.

2. **Two distribution links, one unstable connection.** Users in VLANs 10 and 30 see intermittent connectivity after a maintenance window. Two physical switch links should act as one logical trunk. Find any mismatch in their VLAN permissions or LACP negotiation and verify that both member links carry the expected traffic.

3. **New department on a shared uplink.** A switch pair carries management traffic and two department VLANs, but a downstream trunk should carry only the departments. After an expansion, one department cannot reach shared services. Trace the allowed VLAN lists on both trunks and the LACP bundle before changing the path.

4. **Voice traffic crossing the wrong native VLAN.** A replacement switch comes online with a different native VLAN from its peer. The bundled uplink must carry only the agreed native and voice VLANs. Diagnose the mismatch, align both ends, and confirm that the port channel remains healthy.

5. **Only one member of a redundant uplink is forwarding.** A switch-to-switch connection appears up, yet traffic drops during failover. The links are supposed to carry VLANs 10, 11, and 30 as one LACP bundle, with one switch initiating negotiation. Determine why the second member did not join and verify failover after repair.

### Access ports and neighbor discovery

6. **Desk phones work, but connected PCs do not.** A small office uses separate data and voice VLANs on the same desk ports. After a switch replacement, phones register while PCs lose access. Check the port assignments on both switches and use LLDP to confirm which devices and uplinks are connected.

7. **Operations and engineering desks are crossed.** Two new desks are plugged into the wrong access networks, leaving Operations and Engineering users on the wrong VLANs. Map the physical connections, correct the access ports, and verify the switch-to-switch neighbor information with LLDP.

8. **A user port exposes more discovery information than intended.** Sales and Marketing PCs need their own VLANs, but the Sales desk also receives switch discovery advertisements. Verify both access VLANs, keep LLDP available for network-device links, and stop discovery traffic in both directions on that end-user port.

9. **A phone and PC share an incorrectly configured port.** A new Engineering desk has a phone with a PC connected through it. The two devices need separate voice and data treatment; the router-facing link also has a CDP restriction. Recheck the access configuration and discovery settings without disrupting the uplink.

10. **A voice VLAN is missing after a floor move.** Staff move between two switch closets and phones stop registering although their PCs still connect. Reconcile the data and voice VLAN plan across both switches, then use LLDP to validate the new cabling.

11. **Neighbor discovery works, but an uplink leaks CDP.** A switch pair needs LLDP for inventory while the inter-switch-facing ports must not advertise CDP. Trace the device connections, repair the data and voice VLAN assignments, and check discovery behavior on the specified uplinks.

12. **Sales phones and PCs land in the same network.** A sales office expects separate data and voice VLANs on its desk ports. After replacing both switches, phones fail to register. Confirm the two VLANs, correct the desk ports, and verify LLDP neighbors before closing the incident.

### Dual-stack addressing

13. **A point-to-point circuit is physically up but unreachable.** Two routers on a new circuit cannot ping each other over either protocol. Check the assigned boundary addresses and masks in the planned IPv4 /29 and IPv6 /126 networks, repair any mismatch, and test both directions.

14. **A second router pair was given inconsistent addresses.** The circuit plan calls for the first and last assignable addresses from its IPv4 and IPv6 allocations. Audit both interfaces against the plan and verify IPv4 and IPv6 reachability before routing is enabled.

15. **A remote site is unreachable after renumbering.** The link between two routers was moved to a different IPv4 /29 and IPv6 /126 allocation. Find which endpoint still uses an old address or prefix, bring both ends into the new plan, and confirm neighbor reachability.

16. **A tiny transit network has an addressing error.** A two-router transit link uses IPv4 /30 and IPv6 /126 allocations. A copied configuration makes one side unreachable. Calculate the correct endpoint addresses from the assigned prefixes and confirm both protocol families work.

17. **A larger transit block does not match the design sheet.** The routers were assigned IPv4 /27 and IPv6 /122 prefixes for a new segment, but their interface settings disagree. Compare the actual addresses with the allocation plan and test local and end-to-end connectivity after correction.

### OSPF adjacency and designated-router election

18. **A central router loses two OSPF neighbors.** After a routing change, the central router no longer exchanges routes with two peers on its shared Ethernet segment. Check the intended interfaces and advertised networks, restore adjacency, and verify that the central router wins the designated-router election on that segment.

19. **The designated router changes after maintenance.** A replacement central router joins its OSPF peers but does not become the designated router for the shared network. Check its stable router ID, network statements, and interface priority, then confirm neighbor state and the election result.

20. **A routing cutover leaves one peer isolated.** OSPF was enabled directly on a new router's interfaces, yet only one of two expected neighbors appears. Find the missing adjacency, verify the chosen router ID, and confirm the router becomes designated router on the intended broadcast segment.
