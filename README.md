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

pihole_conditional_forward_server: 77.88.8.8   # Yandex public DNS
pihole_conditional_forward_domains:
  - ru
  - xn--p1ai   # .рф
```

## Conditional DNS forwarding

Queries for the TLDs in `pihole_conditional_forward_domains` are answered by
`pihole_conditional_forward_server` instead of Pi-hole's default upstreams. The
role renders these as `server=/<tld>/<server>` lines into a dnsmasq drop-in
(`02-conditional-forward.conf`) in the bind-mounted `dnsmasq.d` directory and
restarts the container.

By default `.ru` and `.рф` resolve via Yandex DNS (`77.88.8.8`). `.рф` is listed
in **punycode** (`xn--p1ai`), not Cyrillic, because resolvers convert IDN TLDs
to A-labels before the query reaches DNS — a literal `рф` rule would never
match. Set `pihole_conditional_forward_domains: []` to remove the drop-in.
