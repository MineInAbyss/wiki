This page goes over general sysadmin info. We try to be open about these things so other projects can benefit from our work. Most of the projects linked here go into a lot more detail about how they work, be sure to check them out!

## Hosting

We use a dedicated machine on Hetzner for hosting. It’s located in Germany as it's cheaper compared to services like OVH in America.

## Infra

Infra is handled via Docker and Ansible

- We publish our own server image on [MineInAbyss/Docker-images](https://github.com/MineInAbyss/Docker-images) 
- We keep docker-compose stacks at [MineInAbyss/Docker-stacks](https://github.com/MineInAbyss/Docker-stacks)
- We manage machine setup with Ansible at [MineInAbyss/ansible-in-abyss](https://github.com/MineInAbyss/ansible-in-abyss)

## Domain

We use Cloudflare for DNS as well as a registrar. We have a wildcard domain set up to get SSL on all internal services easily with Traefik.
