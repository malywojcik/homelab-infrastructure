Home Lab Infrastructure & Hybrid Cloud

Repository containing Infrastructure-as-Code (IaC) configurations for my self-hosted homelab server. The system is designed with a Zero Trust approach using Cloudflare Tunnel and containerized services via Docker.

Network Diagram
<img width="3720" height="1702" alt="diagram_en" src="https://github.com/user-attachments/assets/9877365e-2ef2-4608-aaef-8bac2d4c2e66" />


Key Configurations:

Cloudflare Tunnel: Secure ingress without open ports.

Minecraft Cluster: High-performance Java server optimized with Folia & Velocity proxy (JVM tuning).

Monitoring: Uptime Kuma for SLA tracking.

Storage: Planned migration to MergerFS + SnapRAID architecture.


Architecture Decisions:

Service Isolation Instead of a monolithic docker-compose.yml, I utilized a distributed configuration strategy. Each user/service (e.g., Minecraft, Portfolio, Monitoring) has its own dedicated directory and docker-compose.yml file.

Why?

Security & Permissions: Allows enforcing strict Linux file permissions (chown/chmod) per directory, ensuring users can only modify their own service configurations without risking the core infrastructure.

Stability: Updating one service (e.g., restarting the Minecraft container) does not affect the uptime of critical services like Monitoring or the Cloudflare Tunnel.
