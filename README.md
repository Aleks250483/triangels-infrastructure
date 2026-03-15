# TriAngels Infrastructure

![License](https://img.shields.io/badge/license-MIT-blue)
![Architecture](https://img.shields.io/badge/architecture-zero--trust-green)
![Network](https://img.shields.io/badge/network-mesh-orange)
![Infrastructure](https://img.shields.io/badge/infrastructure-headscale%20%2B%20tailscale-purple)

Private Zero-Trust Mesh Network

TriAngels — это распределённая частная mesh-сеть, построенная на основе:

- Headscale
- Tailscale
- Zero-Trust архитектуры
- Multi-Tenant инфраструктуры

Сеть предназначена для безопасного соединения устройств, партнёров и инфраструктуры в единую защищённую сеть.

---

# Network Architecture

TriAngels Mesh Network

```text
                Control Plane
             triangels-core-fi-01
                  Headscale
                     │
                     │
        ┌────────────┴────────────┐
        │                         │
   Exit Node                 Exit Node
triangels-exit-hu-01     triangels-exit-fi-02
      Hungary                  Finland
        │                         │
        └────────────┬────────────┘
                     │
                Mesh Clients
      Mac / Windows / Linux / iOS / Android
Infrastructure Components
Control Plane

Headscale server управляет всей сетью.

Server:

triangels-core-fi-01

Функции:

управление узлами

ACL политика

авторизация устройств

управление тегами

Exit Nodes

Exit Nodes позволяют клиентам направлять интернет-трафик через безопасные узлы сети.

Примеры:

triangels-exit-hu-01
triangels-exit-fi-02

Каждая exit node:

подключена к Headscale

имеет тег tag:exit

рекламирует маршрут 0.0.0.0/0

Документация по развёртыванию:

docs/TRIANGELS_EXIT_NODE_DEPLOYMENT.md
Mesh Clients

К сети могут подключаться:

macOS

Windows

Linux

iOS

Android

NAS

VPS

Security Model

TriAngels использует:

Zero-Trust архитектуру

Tag-based ACL

Multi-tenant сегментацию

End-to-End encryption (WireGuard)

Каждое устройство получает:

100.64.0.0/10 mesh IP
Documentation

Infrastructure documentation:

docs/TRIANGELS_EXIT_NODE_DEPLOYMENT.md

Emergency procedures:

TriAngels_VPS_EMERGENCY.md
Future Architecture

В будущем TriAngels будет поддерживать:

Geo Exit Routing

Exit Node Failover

Federated Core Nodes

Partner Infrastructure

Project Goal

Создать распределённую защищённую инфраструктуру для партнёров TriAngels.

Maintainer

TriAngels Infrastructure Team
