# Project-1-Nginx-Reverse-Proxy-Basic-DevOps-Lab-
Set up Nginx as a reverse proxy to forward traffic to a backend server.

So I built a simple setup where:
- Nginx receives requests
- Then forwards them to a backend server

---

## Architecture (simple view)


Browser → Nginx (port 80) → Backend (Python server on port 8000)


---

## Step 1: Install and run Nginx

```bash
sudo apt update
sudo apt install nginx -y

Start the service:

sudo systemctl start nginx

Enable it to start on boot:

sudo systemctl enable nginx

Check if it's running:

sudo systemctl status nginx

At this point, opening http://localhost should show the default Nginx page.

Step 2: Create a simple backend server

I used Python to quickly spin up a backend.

cd ~
nano index.html

Added this:

<h1>Backend Working</h1>
<p>This is coming from the backend server</p>

Start the backend:

python3 -m http.server 8000

Test it:

curl localhost:8000

If it works, I know the backend is ready.

Step 3: Configure Nginx as a reverse proxy

Open the config file:

sudo nano /etc/nginx/sites-available/default

Find this block:

location / {
    try_files $uri $uri/ =404;
}

Replace it with:

location / {
    proxy_pass http://localhost:8000;
}

What this does:

Instead of serving files directly
Nginx forwards all requests to the backend
Step 4: Apply changes

Test config:

sudo nginx -t

Restart Nginx:

sudo systemctl restart nginx
Step 5: Test everything

Open:

http://localhost

If I see my backend page → success.

Also tested with:

curl localhost
What I learned
Nginx can act as a middle layer (reverse proxy)
Requests don’t have to go directly to the app
Services run on ports and must not conflict
Always test things step by step (backend first, then proxy)
Common issues I hit

Port already in use

sudo lsof -i :80

Config errors

sudo nginx -t

Changes not applied

sudo systemctl restart nginx
Final result

I built a simple but real setup where:

Nginx handles incoming traffic
Backend handles the actual content
