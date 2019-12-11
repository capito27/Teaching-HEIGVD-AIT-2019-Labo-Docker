# Task 6

## Déliverables 
###2)

The final solution is still quite clunky, with most of the infrastructure hoding on quite poorly, and with a lot of thing that should not be done being done.
We would really recommend migrating the whole system from haproxy to a docker-compose, traefik based reverse proxy/loadbalanced setup.

It would remove almost all issues and workarounds, as docker-compose would automatically detect downed containers and restart them, as well as traefik autmatically noticing rescaling requests from within docker-compose and updating it's load balancing lists.