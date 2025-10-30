# dokploy-connect-docker-container-commands

🧩 PocketBase Traefik 404 Fix (Dokploy)

If your PocketBase deployment on Dokploy is showing a 404 Not Found error after adding a domain, follow these steps to fix it.

🛠 Step-by-Step Fix
1️⃣ Attach your PocketBase container to the dokploy-network

Run:

docker network connect dokploy-network testing-pocketbase-pocketbase-gzqzhm-pocketbase-1


⚠️ Note:
If your actual container name differs, check it using:

docker ps | grep pocketbase


and use that exact name.

2️⃣ Restart Traefik so it reloads the network configuration

Run:

docker restart dokploy-traefik

3️⃣ Verify the connection

Run:

docker network inspect dokploy-network | grep pocketbase -A 3


✅ You should now see your PocketBase container listed under Containers:

4️⃣ Check if Traefik now detects your domain

Run:

docker exec -it dokploy-traefik traefik show-config | grep test.zepresearch.com -A 5


✅ If you see a router rule for test.zepresearch.com, it’s fixed.

5️⃣ Test the domain

Open in your browser:

https://test.zepresearch.com


Or use:

curl -I https://test.zepresearch.com


✅ You should see a response like:

HTTP/2 200
content-type: text/html
...

❌ Still Getting 404?

Run:

docker logs -f dokploy-traefik | grep test.zepresearch.com


While accessing the domain.

If still nothing appears, it means:

Dokploy didn’t add correct traefik.http.routers.*.rule labels.

Would you like me to show you the exact Traefik labels you should add to your PocketBase deployment so Dokploy always attaches Traefik correctly even after redeploy?

Would you like me to format this README with syntax highlighting, emojis, and collapsible sections (for better GitHub presentation)?


while you access the domain — if still nothing appears, it means:

Dokploy didn’t add correct traefik.http.routers.*.rule labels.
We can verify that next by checking the container labels.

Would you like me to show you the exact labels you need to add to your PocketBase deployment (so Dokploy always attaches Traefik correctly even after redeploy)?
