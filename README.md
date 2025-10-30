# dokploy-connect-docker-container-commands

1️⃣ to indpect netwrok

docker inspect dokploy-network
# 1️⃣ Connect your PocketBase container to the Dokploy network
docker network connect dokploy-network testing-pocketbase-pocketbase-gzqzhm-pocketbase-1

# 2️⃣ Restart the Traefik container to apply network changes
docker restart dokploy-traefik

# 3️⃣ Verify if your domain is linked properly inside Traefik
docker exec -it dokploy-traefik traefik show-config | grep test.zepresearch.com -A 5
