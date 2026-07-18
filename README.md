# pihole server ansible role

Suitable to deploy https://github.com/pi-hole/docker-pi-hole

## Configuration

```yaml
pihole_service_name: pihole
pihole_docker_image: pihole/pihole:2022.10

pihole_network: bridge
pihole_host_pihole_dir: /etc/pihole/pihole
pihole_host_dnsmasq_dir: /etc/pihole/dnsmasq.d
pihole_host_memory: 64m

pihole_port_mappings:
  - 53:53/udp
  - 53:53/tcp
  - 8080:80/tcp

pihole_conditional_forward_server: ""   # upstream IP for the domains below
pihole_conditional_forward_domains: []  # domains/TLDs to route to that upstream
```

## Conditional DNS forwarding

Route queries for specific domains or TLDs to a dedicated upstream instead of
Pi-hole's default resolvers. Set both `pihole_conditional_forward_server` (an
upstream IP) and `pihole_conditional_forward_domains` (a list of domains): the
role renders `server=/<domain>/<server>` lines into a dnsmasq drop-in
(`02-conditional-forward.conf`) in the bind-mounted `dnsmasq.d` directory and
restarts the container. Both are empty by default, so nothing is forwarded
until you set them (typically from inventory).

Write IDN TLDs in **punycode** (A-label) form — resolvers convert IDN TLDs to
A-labels before the query reaches DNS, so a literal Unicode rule would never
match. Clearing either variable removes the drop-in.
