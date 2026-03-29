# Project 1: Nginx Reverse Proxy (My First Real DevOps Lab)

## What I wanted to do
I wanted to understand how real systems handle traffic, not just install tools.

So I built a simple setup where:
- Nginx receives requests
- Then forwards them to a backend server

---

## Architecture
Browser → Nginx (port 80) → Backend (Python server on port 8000)


---

## Step 1: Install and run Nginx

```bash
sudo apt update
sudo apt install nginx -y

sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx

## Step 2: Create backend server
Create a simple file:

cd ~
nano index.html

Add:

<h1>Backend Working</h1>
<p>This is coming from the backend server</p>

Run Python server:

python3 -m http.server 8000

Test backend:

curl localhost:8000

## Step 3: Configure Nginx reverse proxy

sudo nano /etc/nginx/sites-available/default

location / {
    proxy_pass http://localhost:8000;
}

## Step 4: Apply changes
sudo nginx -t
sudo systemctl restart nginx

## What I learned
Nginx can act as a reverse proxy
Backend runs on a separate port
Nginx forwards traffic between user and backend
Services must be tested step by step

## Common issues

Port conflict:

sudo lsof -i :80

Check config errors:

sudo nginx -t

Restart service:

sudo systemctl restart nginx

## Result

A working reverse proxy setup:
Browser → Nginx → Backend server

