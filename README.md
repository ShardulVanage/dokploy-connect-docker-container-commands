# dokploy-connect-docker-container-commands

Perfect — that confirms the key issue:

👉 Your PocketBase container (testing-pocketbase...) is not connected to the dokploy-network,
so Traefik cannot discover or route traffic to it.

That’s exactly why:

curl shows 301 → HTTPS redirect works (Traefik is handling domain).

Browser shows 404 Not Found → Traefik has no route for test.zepresearch.com.

Let’s fix this cleanly 👇

🛠 Step-by-Step Fix
1. Attach your PocketBase container to the dokploy-network

Run:

docker network connect dokploy-network testing-pocketbase-pocketbase-gzqzhm-pocketbase-1


(If your actual container name differs slightly, check it with docker ps | grep pocketbase and use that exact name.)

2. Restart Traefik so it reloads network config
docker restart dokploy-traefik

3. Verify connection

Run:

docker network inspect dokploy-network | grep pocketbase -A 3


✅ You should now see your PocketBase container listed under Containers:.

4. Check if Traefik now detects your domain

Run again:

docker exec -it dokploy-traefik traefik show-config | grep test.zepresearch.com -A 5


✅ If you now see a router rule for test.zepresearch.com, it’s fixed.

5. Test the domain

Now open in browser:

https://test.zepresearch.com


or via curl:

curl -I https://test.zepresearch.com


✅ You should see:

HTTP/2 200
content-type: text/html
...

If still 404

Run:

docker logs -f dokploy-traefik | grep test.zepresearch.com


while you access the domain — if still nothing appears, it means:

Dokploy didn’t add correct traefik.http.routers.*.rule labels.
We can verify that next by checking the container labels.

Would you like me to show you the exact labels you need to add to your PocketBase deployment (so Dokploy always attaches Traefik correctly even after redeploy)?
