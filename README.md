# Arista SPLab

A lab project for Arista EOS network automation, topology generation, and BGP config rendering.

## Overview

This repository contains an Arista network lab built with Containerlab and Ansible. It includes:

- `clab/splab.clab.yml` - Containerlab topology for Arista core and PE routers plus external ASNs.
- `ansible/playbooks/generate_configs.yml` - Generates EOS configs using NetBox BGP session data.
- `ansible/playbooks/pb_push_configs.yml` - Pushes generated configs to devices.
- `ansible/roles/arista_render_config/` - Ansible role that renders Arista EOS templates, including BGP config.
- `arista-splab.drawio` - Diagram of the lab topology.

## Diagram

The main topology diagram is available as:

- `arista-splab.drawio`

![La Topology](clab/arista-splab.svg)


## Networks and ASNs

| Network / Prefix | Description | ASN |
|---|---|---|
| `10.10.0.0/16` | Management fabric for Containerlab nodes | N/A |
| `10.10.10.0/24` | Arista core node management / P nodes | N/A |
| `10.10.11.0/24` | Arista PE node management | N/A |
| `172.21.10.0/24` | Customer / customer1 routed prefix | `65000` (local Arista AS) |
| `172.22.10.0/24` | Customer / customer2 routed prefix | `65000` (local Arista AS) |
| `192.168.100.0/23` | Aggregated internal IPv4 network advertised via BGP | `65000` |
| `10.0.0.0/23` | Aggregated internal IPv4 network advertised via BGP | `65000` |
| `2001:db8:100:10::/64` | IPv6 customer1 prefix advertised via BGP | `65000` |
| `2001:db8:100:20::/64` | IPv6 customer2 prefix advertised via BGP | `65000` |
| `65100` | External ASN for `as1` router | external eBGP peer |
| `65200` | External ASN for `as2` router | external eBGP peer |
| `65300` | External ASN for `as3` router | external eBGP peer |
| `65400` | External ASN for `as4` router | external eBGP peer |

## ASN Layout

- `65500` – Arista core and PE routers inside the lab.
- `65100` / `65200` / `65300` / `65400` – External BGP peers attached to PE routers.

## Usage

1. Start the lab with Containerlab:
   ```bash
   containerlab deploy --topo clab/splab.clab.yml
   ```
2. Import the variables
```bash
  source ansible/.env
```
3. Generate configs from NetBox BGP data:
   ```bash
   ansible-playbook ansible/playbooks/generate_configs.yml
   ```
4. Push configs to devices:
   ```bash
   ansible-playbook ansible/playbooks/pb_push_configs.yml
   ```

## Notes

- BGP config is templated under `ansible/roles/arista_render_config/templates/`.
- This lab includes both IPv4 and IPv6 BGP session examples.
