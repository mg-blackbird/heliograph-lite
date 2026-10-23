---
name: heliograph-lite
description: Write, check and render heliograph scene files with heliograph lite (heliograph.pyz) - one markdown file that becomes a 1080p animated explainer of a network, system, cyber security, radio or space concept. Use when asked to animate or explain a technical idea as a video, or to write or fix a scene file.
---

# heliograph scenes, with heliograph lite

A scene is one markdown file: front matter that places icons on a backdrop and links them, then one line
per beat, `- START-END | actions | caption`. `heliograph.pyz` checks it, names every mistake with its
line and the fix, and renders it. The reference at the end of this file has the format, the rules, a
complete example and every icon, link type, tone, theme and template: use only names from it.

## Steps

1. **Start close.** Copy the template nearest the request
   (`python heliograph.pyz new --template NAME -o NAME.md`). When none is close, start from the example
   in the reference.
2. **Plan.** One idea per scene: 4 to 7 beats, 25 to 50 seconds. List the nodes (the things) and the
   links between them; every hop a packet travels must be a declared link. For each beat decide what
   moves (`flow`, `trip`), what is emphasised (`highlight`, `callout`, `pulse`, `dim`, `note`) and one
   plain caption sentence.
3. **Write the whole file.**
4. **Check:** `python heliograph.pyz check FILE`, until the last line is `RESULT: OK`. Each `ERROR line N`
   says what is wrong and how to fix it: fix them all, then check again. Fix `WARNING` lines when you can
   (move a node, shorten a caption, lengthen a beat). After five checks that still fail, show the user the
   remaining errors.
5. **Polish:** `python heliograph.pyz review FILE` lists anything left with its fix, and suggestions from
   the writing guide (beats of 5 to 8 seconds, short callouts, a takeaway at the end, acronyms spelled
   out). Take the ones that make the scene clearer.
6. **Look:** `python heliograph.pyz preview FILE` writes `NAME.preview.png` beside it, one small frame per
   beat; `python heliograph.pyz frame FILE -t SECONDS` draws one moment full size. Look for anything shown
   before the caption introduces it, and for empty halves of the frame.
7. **Render when asked:** `python heliograph.pyz FILE` (the same as `render FILE`) writes the MP4 beside
   it; `--draft` makes a quick 720p version.

Without a shell (in a chat), write the file from the reference, and ask the user to run step 4 and paste
back what it prints (or `review FILE --prompt`, which also carries the scene).

## What lasts

Nodes, links and zones are on screen for the whole scene, unless a node has `from:`/`until:` or a link has
`from T`/`until T` (seconds: when it appears and goes). Actions, callouts and notes last only their beat.

## Patterns

- **A tunnel over the real path:** `- laptop > gw: tunnel from 12 via ap net`, where `via` names the nodes
  the line is drawn through; then `flow laptop>gw` sends packets along it from 12 seconds.
- **An eavesdropper:** an `intercept` link to them, `flow ap>snoop`, and a callout such as
  `callout snoop "Sees traffic, cannot read it" alert`.
- **A cut or failure:** the link with `until 9`, and a `cross` node with `from: 9` where it broke.
- **Something new arriving:** the node with `from: 12`, introduced by that beat's caption.
- **Places:** `zones: [{label: Office, cols: [5, 6], rows: [1, 2], tone: accent}]` shades and names an area
  of the stage grid.

## Choosing well

- **Backdrop.** `stage` for any diagram, with nodes on a grid (`grid: {cols: 6, rows: 3}` suits most;
  `grid 2 1` is column 2, row 1, and halves and tenths work: `grid 2.5 1` sits between columns 2 and 3);
  `orbits` for satellites in LEO, MEO and GEO; `earth` for one low orbit or the curve of the earth;
  `panels` to compare two options side by side; `race` to compare latencies.
- **Size.** On a stage with up to seven nodes, `icon_scale: 1.6` fills the frame. One cell per node;
  nothing in the title area above y 150 or the caption box below y 600.
- **Words.** Labels name the thing (up to 24 characters) and details add one fact (28). Call a thing the
  same in its label, captions and callouts, and spell out an acronym once, in a detail. Captions are plain
  sentences, at most 20 characters a second of beat, saying what the viewer should notice.
- **Meaning.** Pick icons and link types by meaning: the reference says when to use each. Tone `alert`
  marks threats and untrusted places, `accent` the key point and protected places.
- **More.** `python heliograph.pyz guide layout` gives the positions, layouts that pass and the fix for
  each warning; `guide writing` covers planning beats and captions; `layout stage` draws every position.

## Reference

As `python heliograph.pyz guide brief` prints it:

```text
HELIOGRAPH QUICK REFERENCE (format 1.0)

A scene is ONE markdown file: YAML front matter between --- lines, then a '## Beats' list.
Each beat line is:  - START-END | action; action; ... | Caption sentence.   (actions: - for none)
A '## Notes' section after the beats is for people and is never rendered.

EXAMPLE (a complete, valid scene)
---
heliograph: "1.0"
title: How a request reaches the database
subtitle: Client, load balancer, application, database
duration: 20
backdrop: stage
grid: {cols: 6, rows: 3}
nodes:
  user: {icon: laptop, at: grid 1 2, label: Analyst, detail: head office}
  lb:   {icon: loadbalancer, at: grid 3 2, label: Load balancer}
  app:  {icon: server, at: grid 4 2, label: App server}
  db:   {icon: database, at: grid 6 2, label: Database}
links:
  - user > lb: api arrow
  - lb > app: api
  - app > db: data
---

## Beats
- 0-6 | flow user>lb; highlight lb | The analyst's browser sends a request to the load balancer.
- 6-12 | flow lb>app>db; callout db "Reads one row" | The balancer picks an app server, which queries the database.
- 12-20 | trip user>lb>app>db>app>lb>user; note "One round trip, about 40 ms" | The answer travels back the same way, hop by hop.

RULES
1. Beats run back to back from 0 to the duration: no gaps, no overlaps, each at least 2.5 s.
2. Captions are plain sentences; at most 20 characters per second of beat time (a 5 s beat fits 100 characters).
3. Every hop of a flow or trip route must be a declared link (either direction).
4. Node ids are lowercase letters, digits and _ and start with a letter. Labels up to 24 characters, details 28.
5. Quote callout and note text with double quotes. In YAML, quote any value that contains ': ' or a comma inside { }.
6. Square brackets in the syntax only mark optional parts; never type them: write callout db "Reads one row" accent, not [tone: accent], and trip a>b every=3, not [every=3].
7. At most 3 callouts and 1 note per beat. Callouts <= 48 chars, notes <= 90.
8. Only use icon names, link types, tones and themes from the lists below (or add them to the library first).
9. Keep content between y 150 and y 600: the title sits above, the caption box below.
10. Leave room between nodes: different grid cells on a stage, 120 apart on the same line elsewhere.
11. No icon fits? Use the closest one and name it in the label, or draw one in the scene's icons: list.

BACKDROPS AND POSITIONS (node field at:)
  stage:  grid C R (default 6 x 4; grid: {cols: 6, rows: 3} suits most), or X Y anywhere
          background: dots (default), plain, grid, mesh or stars
  orbits: ground X (x 40-1240 on the curved surface), leo X, meo X, geo X (on that orbit shell), sky X Y
  earth:  ground X (x 40-1240 on the curved surface), orbit X (the one low orbit), sky X Y (free)
  panels: ground X / upper X (orbit X means the same) / sky X Y; left panel x 30-620, right x 660-1250
  race:   no nodes; lanes of tracks instead (RACE BACKDROP in guide format)
  map:    latlon LAT LON (north and east positive), sky X Y (fixed on screen); views: and view NAME zoom

ACTIONS (inside a beat, separated by ;)
  flow a>b>c [count=N] [period=S] [on=TYPE|ID]                         continuous packets along a route; every hop must be a declared link (on= picks a link by type or id)
  trip a>b>c [every=S] [delay=S] [on=TYPE|ID]                          packets travel the route hop by hop (about 650 px a second), a new one every S seconds (by default about 0.6 times the trip's travel time); going back over a hop draws it offset (a hairpin)
  highlight x [y ...]                                                  emphasise nodes for this beat
  pulse x                                                              pulsing alert halo on a node for this beat
  callout x "text" [tone] [place=above|below|left|right] [on=TYPE|ID]  label pill on a node, or on a link written a>b; placed on the clearest side unless place= is given
  note "text" [tone]                                                   one note pill for this beat (top centre of the frame, over the focused panel, or under the race)
  footprint s [t ...]                                                  coverage cone under a node (usually a satellite) for this beat
  dim x [y ...]                                                        de-emphasise nodes and their links for this beat
  move x to POSITION [in=S]                                            glide a node to a new position (same syntax as at:) and leave it there
  tab N                                                                select tab N (counting from 1) from the front-matter tabs
  focus left|right                                                     panels only: dim the other panel for this beat
  view NAME [in=S]                                                     map: pan and zoom to a named view

LINKS:  - a > b: TYPE [buried] [straight] [from T] [until T] [via node node ...] [bend N] [arrow] [label "text"] [id NAME]
        (from/until: seconds it appears and goes; via: nodes the line is drawn through, such as a tunnel
        over the real path; id: the name on= picks)
NODES:  id: {icon, at, label, detail, tone, scale, label_pos, label_dx, drift, from, until}
FRONT MATTER: heliograph, about, tags, eyebrow, title, subtitle, duration, backdrop, theme, icon_scale, stamp, readout, source, legend, tabs, background, seed, grid, shells, surface, orbit_label, constellation, raise, views, zones, labels, panels, regions, lanes, race_seconds, nodes, links, icons, marking

ICONS (158; common aliases also work, e.g. db, building, plane; descriptions: python heliograph.pyz guide icons)
  people: user, team, operator, partner, house, office, city, factory, government, securesite, globe, pin, flag, portcrane
  compute: desktop, laptop, phone, sensor, camera, plc, server, rack, database, storage, cluster, container, function, api, queue, chip, ai, browser, code, document, tasking, briefing, mail, chat, language, domaincontroller, updateserver, harddrive
  network: router, switch, firewall, accesspoint, loadbalancer, dns, sdwan, cloud, pop, tower, cablelanding, solar, battery, generator
  security: lock, key, shield, certificate, mfa, badge, vault, hsm, vpn, proxy, waf, edr, sandbox, honeypot, bastion, nac, encryptor, decrypt, cds, diode, attacker, malware, ransomware, phishing
  monitoring: eye, tap, span, packetbroker, ids, pcap, logs, siem, soc, linkchart
  optical: opticaltransponder, mux, demux, amplifier, roadm, patchpanel, sfp, subsearepeater, olt, splitter, ont, otdr, qkd, fso, observatory, heliograph, signallamp
  space: satellite, geosat, meosat, smallsat, relaysat, rocket, dish, flatpanel, satphone, gateway, pad, antenna, groundstation
  radio: lpda, dfarray, df, radome, remotesite, receiver, spectrum, waterfall, dsp, dffix
  defence: aircraft, isrjet, helicopter, uav, drone, vehicle, ship, submarine, radar, buoy, commandpost, radio, emitter, jammer
  generic: box, node, hexagon, diamond, star, check, cross, warning, clock, gear, bell, chart, gauge, magnifier

LINK TYPES (name - legend text (how it looks) - when to use)
  data      - Data flow (solid white) - generic data or IP traffic between systems
  api       - API call (dashed pale pink) - request and response between services (HTTP, gRPC, REST)
  ethernet  - LAN / Ethernet (solid blue) - local wired network: patch, LAN or campus cable
  fibre     - Fibre (solid green) - terrestrial fibre, backhaul or cross-connect
  stream    - Event stream (dotted green) - messages or events through a queue, bus or broker
  control   - Control plane (thin dotted pale yellow) - management, signalling or command traffic between systems
  tunnel    - Encrypted tunnel (dotted orange) - VPN or encrypted overlay; usually drawn with via over the real path
  secure    - Accredited link (thin double red) - classified, accredited or cross-domain connection (double line)
  wifi      - Wi-Fi (wavy pale green) - wireless LAN between a device and an access point
  cellular  - Cellular 4G/5G (wavy pale pink) - mobile network between a device and a cell tower
  mesh      - Mesh radio (wavy pale purple) - tactical or ad hoc radio mesh between moving nodes
  radio     - Radio link (dash-dot white) - terrestrial radio, microwave or HF/UHF/VHF link
  mirror    - Mirrored copy (dash-dot pale blue) - passive copy of traffic from a TAP or SPAN port to a sensor, broker or recorder
  logs      - Log / alert feed (dotted pale teal) - logs, alerts and telemetry sent to a collector, SIEM, SOC or a person on call
  intercept - Interception (dashed pale pink) - someone copying or reading traffic they should not see: Wi-Fi sniffing, a tapped line, a man in the middle
  attack    - Malicious traffic (thick solid red) - attacker traffic, an exploit, a malware callback or exfiltration (awareness and threat scenes)
  satcom    - Satellite link (dashed yellow) - generic satellite link: GEO, MEO or military satcom
  ku        - Ku-band user link (dashed orange) - user terminal (dish, ship, aircraft) to satellite
  ka        - Ka-band gateway link (dashed teal) - satellite to a gateway or gateway-class antenna
  laser     - Laser link (solid pale purple) - between satellites, or to an optical terminal or ground station
  subsea    - Subsea cable (solid blue) - undersea cable; drawn along the sea floor on earth scenes
  bearing   - Line of bearing (thin dashed yellow) - direction-finding bearing from a DF site toward a transmitter; bearings from several sites cross at the fix
  skywave   - HF skywave (wavy pale orange) - HF radio path reflected off the ionosphere, reaching beyond line of sight; add bend to arch it
  power     - Power feed (thick solid orange) - electrical power supply

TONES (for callout, note, node, zone, label): ink, muted, alert, accent, data, api, ethernet, fibre, stream, control, tunnel, secure, wifi, cellular, mesh, radio, mirror, logs, intercept, attack, satcom, ku, ka, laser, subsea, bearing, skywave, power

THEMES (front matter theme:; default midnight)
  midnight - Dark navy ops-console look; the default, made for screens and video.
  blueprint - Engineering-drawing blue with white linework and a yellow accent.
  contrast - Pure black background, white ink and saturated links, for accessibility and projectors.
  daylight - Light background with dark ink, for documents, slides and printing.
  ops - Black operations-room screen with green linework and amber alerts.

TEMPLATES (copy one: python heliograph.pyz new --template NAME -o scenes/my-scene.md)
  stage-starter [stage] - The smallest useful stage scene
  network-overview [stage] - An organisation's network at a glance
  request-response [stage] - A web request through firewall, load balancer, app servers, cache and database, and back
  layered-architecture [stage] - A three-tier architecture drawn as horizontal layers, with a request going down and back up
  data-pipeline [stage] - Events from many sources flow through an ingest API, a queue and processing into storage and a dashboard
  failover [stage] - The primary fibre is cut, an alert fires and traffic reroutes over a satellite backup
  before-after [panels] - Before and after network segmentation
  phishing-response [stage] - A phishing email reaches staff, the endpoint agent blocks the malware's callback, and the SOC contains the incident
  security-zones [stage] - Data crossing from the internet through a DMZ into a protected enclave over a one-way data diode
  network-monitoring [stage] - How a security team sees the network without touching it
  dwdm [stage] - Three services share one fibre, each on its own wavelength
  fibre-to-the-home [stage] - Fibre to the home over a passive optical network
  orbit-layers [orbits] - LEO, MEO and GEO compared
  bent-pipe [earth] - The bent-pipe path from a Starlink dish to its home PoP, with a satellite handover
  laser-relay [earth] - Over the ocean a satellite has no gateway in view, so it relays by laser to one that does
  latency-race [race] - A latency race between cable and an all-laser orbital path over a short and a long distance
  hub-and-spoke [stage] - A headquarters hub reaching a ship, an office, a vehicle and a drone over four different bearers
  mesh-network [stage] - Vehicles in a radio mesh keep talking when one loses its direct link, by relaying through the others
  sensor-to-decision [stage] - Several sensors feed a fusion engine that builds one common picture for a commander
  hf-skywave [earth] - HF skywave
  supply-chain-attack [map] - A poisoned update spreads over a flat global network; an offline backup saves the day
  (what each one shows: python heliograph.pyz guide templates)

MORE: python heliograph.pyz guide format | actions | icons | links | themes | templates | library | workflow | layout | writing
```
