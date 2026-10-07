# 🎮 Public Game Lab

A self-hosted public game environment built to learn networking, containers, reverse proxies, HTTPS, web hosting, and application deployment by solving a real problem:

**Giving my family a simple place to meet online and play games together.**

This project started with a simple idea:

> **Build something fun, make it useful, and use the process to learn how the technology underneath it actually works.**

Rather than building an isolated lab that I would tear down after completing a tutorial, I wanted to create something my family could actually use.

That gave every technical decision a purpose.

The result is **Public Game Lab**, a self-hosted gaming environment running inside my home lab and securely accessible from the public Internet.

---

# 🎯 Project Goals

The primary goal was not simply to host a game server.

The game server became the use case for learning how multiple infrastructure components work together as a complete system.

I wanted hands-on experience with:

- Network design
- Public Internet access
- Network Address Translation (NAT)
- Port forwarding
- Firewalls
- Dynamic Domain Name System (DDNS)
- Domain Name System (DNS)
- Docker containers
- Linux server administration
- Reverse proxies
- HTTP and HTTPS
- Transport Layer Security (TLS)
- Certificates
- Simple HTML/CSS
- Web application hosting
- Application troubleshooting
- Secure service exposure
- Infrastructure documentation

The project also gave me an opportunity to work through a question that sounds simple but involves a surprising amount of infrastructure:

> **How do I take an application running inside a container on a private home network and safely make it usable by family members anywhere on the Internet?**

---

# 🧠 Design Philosophy

I built this project around the infrastructure I already owned.

Instead of purchasing cloud hosting or designing around resources I did not have, I wanted to understand what could be accomplished with my existing home lab.

My environment already included:

- Proxmox virtualization
- pfSense
- Multiple internal lab networks
- An Ubuntu server
- A consumer Internet connection
- An upstream consumer router
- Existing server hardware

That created some constraints, but those constraints became part of the learning experience.

For example, the game server sits behind both pfSense and an upstream consumer router.

That meant inbound traffic initially had to traverse **double Network Address Translation (NAT)**.

Rather than avoiding the problem, I used it to learn exactly how an unsolicited Internet connection reaches a service several network layers deep.

---

# 🏗️ Final Architecture

The final architecture looks like this:

```text
                         Internet
                            │
                        HTTPS :443
                            │
                            ▼
                    Consumer Router
                            │
                            ▼
                         pfSense
                            │
                            ▼
                  Ubuntu Game Server
                     10.10.10.107
                            │
                          Caddy
                     Reverse Proxy
                            │
            ┌───────────────┼───────────────┐
            │               │               │
            ▼               ▼               ▼
      BamBam Labs         Rumpus          GameNest
      Landing Page
            │               │               │
bambam-labs.duckdns.org     │               │
                            │               │
                  rumpus.bambam-labs.       │
                     duckdns.org            │
                                            │
                                gamenest.bambam-labs.
                                      duckdns.org
                            │
                    ┌───────┴───────┐
                    │               │
               Docker :3000    Docker :3001
```

Only the services that need to be publicly reachable are intentionally exposed.

The Docker application ports remain behind the reverse proxy rather than becoming the primary public-facing interface.

---

# 🐳 The Game Server

The Ubuntu game server currently hosts two game applications.

## Rumpus

Rumpus runs inside a Docker container.

```text
Host TCP 3000
      │
      ▼
Rumpus Container
TCP 3000
```

Public access is provided through:

```text
https://rumpus.bambam-labs.duckdns.org
```

## GameNest

GameNest runs in a separate Docker container.

```text
Host TCP 3001
      │
      ▼
GameNest Container
TCP 3000
```

Public access is provided through:

```text
https://gamenest.bambam-labs.duckdns.org
```

Both containers can internally use TCP port 3000 because containers have their own network namespaces.

The Ubuntu host differentiates them using separate host ports.

This was one of the practical ways this project helped reinforce the distinction between a **container port** and a **host port**.

---

# 📦 Learning Docker Through a Real Use Case

Before this project, containers were something I understood conceptually.

This project made them practical.

Instead of installing multiple applications and their dependencies directly onto one Ubuntu operating system, each game application can run within its own containerized environment.

```text
Ubuntu Host
│
├── Rumpus Container
│
└── GameNest Container
```

Each application can have its own dependencies and internal configuration while sharing the same physical/virtual server.

The project provided practical experience with:

- Container images
- Running containers
- Port mappings
- Container networking
- Container filesystems
- Entering running containers
- Inspecting application files
- Troubleshooting applications inside containers

---

# 🌐 Phase 1 — Making a Private Server Public

The first major challenge was simply proving that a service inside the lab could be reached from the Internet.

The network path looked like this:

```text
Internet
   │
   ▼
Consumer Router
   │
   ▼
pfSense WAN
192.168.1.179
   │
   ▼
pfSense LAN
10.10.10.1
   │
   ▼
GAME-SRV
10.10.10.107
```

Because the consumer router and pfSense were both performing Network Address Translation, this created a **double-NAT environment**.

For the initial proof of concept, the game ports were forwarded through both layers.

### Rumpus

```text
Internet TCP 3000
       │
       ▼
Consumer Router
       │
       ▼
pfSense
       │
       ▼
10.10.10.107:3000
       │
       ▼
Rumpus
```

### GameNest

```text
Internet TCP 3001
       │
       ▼
Consumer Router
       │
       ▼
pfSense
       │
       ▼
10.10.10.107:3001
       │
       ▼
GameNest
```

Testing from a phone with Wi-Fi disabled provided an important validation step.

If the application worked over cellular data, the traffic was actually coming from the public Internet rather than simply working because the phone was connected to the local network.

That turned a basic connectivity test into a practical lesson in NAT, routing, firewalls, and service exposure.

---

# 🔐 Phase 2 — Stop Exposing Applications Directly

Direct port forwarding proved that the architecture worked.

It was not the architecture I wanted to keep.

I did not want family members remembering addresses such as:

```text
PUBLIC-IP:3000
PUBLIC-IP:3001
```

I also did not want every application to require another publicly exposed application port.

Instead, I introduced a **reverse proxy**.

I selected **Caddy**.

Caddy gave the project:

- A single HTTPS entry point
- Reverse proxy functionality
- Automatic TLS certificate management
- HTTP-to-HTTPS redirection
- Simple configuration
- The ability to route multiple applications through one server

The public architecture could now center around standard HTTPS traffic on TCP port 443.

---

# 🌎 Dynamic DNS

A residential public IP address may change.

Hard-coding the public IP into bookmarks or giving it to family members would therefore be unreliable.

I used **DuckDNS** to provide a stable hostname that could follow the public IP address.

The primary site became:

```text
bambam-labs.duckdns.org
```

This introduced another useful infrastructure layer:

```text
Human-friendly hostname
        │
        ▼
DNS resolution
        │
        ▼
Current public IP
        │
        ▼
Home network
```

pfSense handles the Dynamic DNS update so the hostname can continue pointing toward the home network if the Internet Service Provider (ISP) changes the public address.

---

# 🔒 HTTPS and Caddy

Caddy became the public-facing web server and reverse proxy.

The final conceptual configuration is:

```caddy
bambam-labs.duckdns.org {
    root * /var/www/bambam
    file_server
}

rumpus.bambam-labs.duckdns.org {
    reverse_proxy localhost:3000
}

gamenest.bambam-labs.duckdns.org {
    reverse_proxy localhost:3001
}
```

Caddy handles the external HTTPS connection and forwards the request to the appropriate internal application.

For example:

```text
Family Member's Phone
        │
        │ HTTPS
        ▼
rumpus.bambam-labs.duckdns.org
        │
        ▼
Public IP
        │
        ▼
Consumer Router
        │
        ▼
pfSense
        │
        ▼
Caddy
        │
        ▼
localhost:3000
        │
        ▼
Rumpus Container
```

The user never needs to know that TCP port 3000 exists internally.

---

# 🧪 A Design That Failed — And Why

One of the most useful parts of the project came from a design that initially looked cleaner.

I originally attempted to host the applications as paths underneath one hostname:

```text
bambam-labs.duckdns.org/rumpus/
bambam-labs.duckdns.org/gamenest/
```

Caddy could easily proxy those paths to the correct containers.

However, the applications themselves were not designed to operate from those subdirectories.

For example, GameNest requested resources using paths such as:

```text
/style.css
```

rather than:

```text
/gamenest/style.css
```

The result was interesting.

GameNest's HTML loaded but initially appeared without styling.

After routing the stylesheet, additional application resources still failed and the game cards disappeared.

Rumpus produced a white page.

The reverse proxy itself was working.

The problem was **application path awareness**.

Both applications assumed they were running at the root:

```text
/
```

Trying to solve the problem by individually proxying CSS, JavaScript, images, Application Programming Interfaces (APIs), WebSockets, and other paths would create a fragile configuration.

Instead, I changed the architecture.

---

# 🧭 Subdomains Instead of Subdirectories

Each application received its own hostname:

```text
bambam-labs.duckdns.org
rumpus.bambam-labs.duckdns.org
gamenest.bambam-labs.duckdns.org
```

Now every application can operate from `/`, exactly as it expects.

```text
rumpus.bambam-labs.duckdns.org/
                 │
                 ▼
              Rumpus /
```

and:

```text
gamenest.bambam-labs.duckdns.org/
                 │
                 ▼
             GameNest /
```

This eliminated the root-relative asset and routing problems.

This became one of the most valuable architecture lessons from the project:

> **A reverse proxy can route a request anywhere, but the application behind it still has assumptions about where it lives. Infrastructure design has to account for application behavior.**

---

# 🏠 BamBam Labs Landing Page

Once the individual applications had clean public URLs, I wanted one simple front door for the family.

That became:

```text
https://bambam-labs.duckdns.org
```

The landing page is intentionally simple HTML/CSS.

It provides links to the available game systems without requiring anyone to remember individual hostnames.

```text
              BamBam Labs
                  │
          ┌───────┴───────┐
          │               │
     Play Rumpus      Open GameNest
          │               │
          ▼               ▼
   rumpus.bambam...  gamenest.bambam...
```

Building the landing page also gave me a practical introduction to basic HTML and CSS.

I wasn't trying to become a front-end developer through this project.

I wanted enough understanding to create a functional interface that connected users to the infrastructure I had built.

---

# 🇺🇸 GameNest Localization

GameNest originally defaulted to Chinese.

Because this deployment is primarily intended for my family, I changed the application's default language to English while retaining the existing language functionality.

Investigating the problem required entering the running container, locating the application's language files, and tracing how the JavaScript selected its default language.

The relevant client-side initialization was changed from:

```javascript
if (!window.__ACTIVE_LANG) window.__ACTIVE_LANG = 'zh';
```

to:

```javascript
if (!window.__ACTIVE_LANG) window.__ACTIVE_LANG = 'en';
```

The existing English language implementation already supported `en`.

This was a small change, but it provided another useful lesson:

> **Containers are not black boxes. When troubleshooting requires it, I can inspect the application, understand how it behaves, identify the smallest appropriate change, and validate the result.**

---

# 🔬 Testing Methodology

I tried not to treat **"the webpage appeared"** as sufficient proof that something worked.

Different layers were tested independently.

## Confirm Docker Containers and Ports

```bash
docker ps
```

## Confirm Local Application Availability

```bash
curl -I http://localhost:3001/
```

## Confirm Application Assets

```bash
curl -I http://localhost:3001/style.css
```

## Confirm Listening Ports

```bash
ss -lntp
```

## Confirm DNS

```bash
nslookup bambam-labs.duckdns.org
nslookup rumpus.bambam-labs.duckdns.org
nslookup gamenest.bambam-labs.duckdns.org
```

## Validate Caddy Before Applying Changes

```bash
sudo caddy validate --config /etc/caddy/Caddyfile
```

## Test From Outside the Network

The applications were tested from a mobile phone using cellular data.

This helped separate:

```text
Application problem
        vs.
Docker problem
        vs.
Linux problem
        vs.
Reverse proxy problem
        vs.
Firewall/NAT problem
        vs.
DNS problem
        vs.
Internet accessibility problem
```

That troubleshooting mindset became as important as the final configuration.

---

# 🛡️ Security Approach

Making something publicly accessible changes the security model.

The goal was therefore not simply:

> **Make the game server reachable.**

The goal was:

> **Expose only what needs to be reachable while keeping the rest of the lab private.**

The architecture evolved away from directly exposing individual application ports toward a controlled HTTPS entry point.

The intended public path is:

```text
Internet
   │
   │ HTTPS :443
   ▼
Firewall / NAT
   │
   ▼
Caddy
   │
   ├── BamBam Labs
   ├── Rumpus
   └── GameNest
```

The rest of the Ubuntu server and internal lab do not need to become public simply because these applications are public.

Future hardening work will continue to improve:

- Network segmentation
- Access control
- Logging
- Patch management
- Backup strategy
- Exposure reduction
- Monitoring

---

# 💾 Known-Good State

After completing the initial deployment, the environment was validated from an external cellular connection.

Confirmed working:

- BamBam Labs landing page
- HTTPS
- DuckDNS resolution
- Caddy reverse proxy
- Rumpus public subdomain
- GameNest public subdomain
- Rumpus gameplay
- GameNest game catalog
- GameNest styling/assets
- GameNest English default
- Landing-page navigation
- Docker containers
- External cellular access

A Proxmox snapshot was taken at this milestone before further development.

```text
Known-Good-Public-Gaming-2026-10-06
```

This provides a rollback point before future experimentation.

> **Note:** A Proxmox snapshot is a rollback mechanism, not a replacement for a proper backup strategy.

---

# 💡 What This Project Actually Taught Me

The biggest lesson was not any individual command or technology.

It was seeing how the pieces depend on one another.

A user tapping **Play Rumpus** looks incredibly simple.

Behind that button is something closer to:

```text
HTML
 ↓
DNS
 ↓
Public IP
 ↓
Consumer Router
 ↓
NAT
 ↓
pfSense
 ↓
Firewall Rules
 ↓
Linux
 ↓
Caddy
 ↓
TLS
 ↓
Reverse Proxy
 ↓
Docker Networking
 ↓
Container
 ↓
Web Application
 ↓
WebSocket / Application State
```

The finished experience hides almost all of that complexity.

That is exactly what good infrastructure should do.

The user should not need to understand my network architecture to play a game.

**I do.**

---

# Why This Project Matters to Me

This project reflects the way I prefer to learn.

I can read about Docker, reverse proxies, TLS, NAT, or DNS independently, but understanding becomes much deeper when those technologies have to cooperate to produce something real.

Public Game Lab gave each technology a reason to exist.

Docker wasn't installed just to learn Docker.

Caddy wasn't installed just to learn reverse proxies.

DuckDNS wasn't configured just to learn Dynamic DNS.

pfSense wasn't configured just to practice firewall rules.

HTML wasn't written just to practice HTML.

Each component solved a problem created by the project before it.

That created a natural progression:

```text
I want my family to play a game remotely.
                ↓
The game needs a server.
                ↓
I need somewhere to run it.
                ↓
Containers provide application isolation.
                ↓
The containers need networking.
                ↓
The server needs to become reachable externally.
                ↓
Double NAT must be understood.
                ↓
Direct ports work, but aren't the design I want.
                ↓
I need a reverse proxy.
                ↓
Public traffic should use HTTPS.
                ↓
My public IP can change.
                ↓
I need Dynamic DNS.
                ↓
Multiple applications need clean routing.
                ↓
Path routing conflicts with application design.
                ↓
Subdomains provide cleaner application boundaries.
                ↓
My family needs an easy way to find everything.
                ↓
Build a simple landing page.
```

That progression is what makes this more than a game server.

It is a small example of **designing infrastructure around a purpose, discovering constraints, testing assumptions, learning from failures, and evolving the architecture until the technology becomes almost invisible to the people using it.**

---

# 🚀 Future Development

Public Game Lab is intentionally an evolving project.

Potential future work includes:

- Stronger network segmentation
- Dedicated game-server VLAN
- Improved firewall policy
- Automated container deployment
- Docker Compose
- Persistent-data backup strategy
- Proxmox backup integration
- Centralized logging
- Monitoring and alerting
- Container health monitoring
- Automated updates with controlled testing
- Additional games
- Improved landing-page design
- Service status indicators
- Infrastructure-as-Code experimentation
- Documented disaster recovery
- Further Linux hardening

The goal is not to add technology simply because it exists.

New components should solve a problem, improve security/reliability, or provide a worthwhile learning opportunity.

---

# 📊 Project Status

**Status:** 🟢 Operational

## BamBam Labs

```text
https://bambam-labs.duckdns.org
```

## Rumpus

```text
https://rumpus.bambam-labs.duckdns.org
```

## GameNest

```text
https://gamenest.bambam-labs.duckdns.org
```

The environment is operational and has been validated from outside the local network.

---

# 🔐 Documentation Security Note

Security was considered when deciding what information to publish in this repository.

The internal IP addresses shown in this documentation are **private, non-Internet-routable addresses** used to explain the lab architecture.

Current public IP addresses, credentials, passwords, API tokens, private keys, sensitive certificate material, and other secrets are intentionally excluded.

The goal of this repository is to document **how the system was designed and how the technologies work together without publishing information that would unnecessarily increase the exposure of the live environment.**

---

# Final Thought

What began as:

> **"Let's host some games for the family."**

became an exercise in:

**Networking → Virtualization → Linux → Docker → DNS → TLS → Reverse Proxies → HTML → Troubleshooting → Security → Architecture**

That's exactly why I built it.

**The games are the product my family sees.**

**The infrastructure behind them is the project I wanted to learn.**
