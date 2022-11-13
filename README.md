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
```
