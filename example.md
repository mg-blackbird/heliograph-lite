---
heliograph: "1.0"
about: An organisation's network at a glance - offices and remote staff reach cloud apps and the data centre through one secure gateway, and the security team sees every connection.
tags: [network, overview, cloud, remote, gateway, sase, zero-trust, security, mesh]
title: One network, many places
subtitle: Offices and remote staff reach the cloud through one secure gateway
duration: 30
backdrop: stage
background: mesh
seed: 3
grid: {cols: 6, rows: 3}
nodes:
  hq:     {icon: office, at: grid 1 1.2, label: Head office}
  branch: {icon: office, at: grid 1 2.1, label: Branch office}
  remote: {icon: laptop, at: grid 1 3, label: Remote staff}
  net:    {icon: cloud, at: grid 2.6 2.1, label: Internet}
  gw:     {icon: firewall, at: grid 4 2.1, label: Secure gateway}
  apps:   {icon: container, at: grid 5.6 1.2, label: Cloud apps}
  dc:     {icon: rack, at: grid 5.6 3, label: Data centre}
  soc:    {icon: soc, at: grid 4 3.2, label: Security team}
links:
  - hq > net: fibre
  - branch > net: fibre
  - remote > net: wifi
  - net > gw: tunnel
  - gw > apps: data
  - gw > dc: data
  - gw > soc: logs
---

## Beats
- 0-7 | flow hq>net>gw>apps; highlight gw | Staff at head office reach cloud apps over the internet, and every connection passes through one secure gateway.
- 7-13 | flow branch>net>gw>dc | The branch office takes the same path to reach systems in the data centre.
- 13-20 | flow remote>net>gw>apps; callout remote "Same checks at home" | Remote staff get the same checks wherever they work, from home, a hotel or an airport.
- 20-25 | flow gw>soc; highlight soc; callout gw "Logs every connection" | The gateway logs every connection, so the security team sees the whole network.
- 25-30 | trip remote>net>gw>apps>gw>net>remote; note "One set of rules, one view" accent | One set of rules and one view, for every place people work.
