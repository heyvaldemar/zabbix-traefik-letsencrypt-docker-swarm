# Zabbix with Let's Encrypt in a Docker Swarm

Configure Traefik and create secrets for storing the passwords on the Docker Swarm manager node before applying the configuration.

Traefik configuration: https://github.com/heyValdemar/traefik-letsencrypt-docker-swarm

Create a secret for storing the password for Zabbix database using the command:

`printf "YourPassword" | docker secret create zabbix-postgres-password -`

Clear passwords from bash history using the command:

`history -c && history -w`

Run `zabbix-restore-database.sh` on the Docker Swarm node where the container for backups is running to restore database if needed.

Run `docker stack ps zabbix | grep zabbix_backups | awk 'NR > 0 {print $4}'` on the Docker Swarm manager node to find on which node container for backups is running.

Deploy Zabbix in a Docker Swarm using the command:

`docker stack deploy -c zabbix-traefik-letsencrypt-docker-swarm.yml zabbix`
