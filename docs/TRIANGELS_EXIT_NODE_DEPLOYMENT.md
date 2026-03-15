TRIANGELS_EXIT_NODE_DEPLOYMENT.md
TriAngels Mesh Infrastructure
Exit Node Deployment Standard
Version: 1.0 Network: TriAngels Private Mesh Architecture: Headscale + Tailscale Model: Zero-Trust Multi-Tenant Mesh

1. Overview
Exit Node — это узел TriAngels Mesh, через который клиенты могут направлять свой интернет-трафик.
Exit Nodes используются для:
безопасного выхода в интернет
обхода сетевых ограничений
маршрутизации трафика партнёров
географического распределения трафика
отказоустойчивости сети
Каждая Exit Node:
подключена к Headscale Control Plane
получает mesh IP (100.64.0.0/10)
имеет тег tag:exit
рекламирует маршрут 0.0.0.0/0

2. TriAngels Network Architecture

TriAngels Mesh Network

                   Control Plane
                triangels-core-fi-01
                      Headscale
                       │
                       │
        ┌──────────────┴──────────────┐
        │                             │
   Exit Node                     Exit Node
triangels-exit-hu-01         triangels-exit-fi-02
    Hungary                      Finland
        │                             │
        └──────────────┬──────────────┘
                       │
                   Mesh Clients
        Mac / Windows / Linux / iOS / Android


3. Exit Node Requirements
Минимальные требования VPS:


4. Naming Convention
Exit nodes должны использовать следующий формат имени:

triangels-exit-<country>-<number>

Примеры:

triangels-exit-hu-01
triangels-exit-fi-02
triangels-exit-de-01


5. Step 1 — VPS Preparation
Подключаемся к серверу:

ssh root@SERVER_IP

Обновляем систему:

apt update && apt upgrade -y

Устанавливаем базовые утилиты:

apt install curl sudo ufw nano -y


6. Step 2 — Set Hostname
Назначаем hostname:

hostnamectl set-hostname triangels-exit-fi-02

Проверяем:

hostnamectl


7. Step 3 — Enable IP Forwarding
Открываем sysctl:

nano /etc/sysctl.conf

Добавляем:

net.ipv4.ip_forward=1
net.ipv6.conf.all.forwarding=1

Применяем:

sysctl -p

Проверяем:

sysctl net.ipv4.ip_forward

Ожидаемый результат:

net.ipv4.ip_forward = 1


8. Step 4 — Install Tailscale
Установка:

curl -fsSL https://tailscale.com/install.sh | sh

Проверяем:

tailscale version

Проверяем сервис:

systemctl status tailscaled


9. Step 5 — Create Headscale Auth Key
На Core сервере выполняем:

sudo headscale preauthkeys create \
--user USER_ID \
--tags tag:exit \
--expiration 24h

Пример:

sudo headscale preauthkeys create \
--user 1 \
--tags tag:exit \
--expiration 24h

Получаем ключ:

hskey-auth-XXXXXXXX


10. Step 6 — Connect Exit Node
На Exit Node выполняем:

tailscale up \
--login-server=https://HEADSCALE_SERVER \
--auth-key=AUTH_KEY \
--hostname=NODE_NAME \
--advertise-exit-node \
--accept-dns=false \
--reset

Пример:

tailscale up \
--login-server=https://89.125.59.191.nip.io \
--auth-key=hskey-auth-XXXXXXXX \
--hostname=triangels-exit-fi-02 \
--advertise-exit-node \
--accept-dns=false \
--reset


11. Step 7 — Verify Node Registration
На Core сервере:

headscale nodes list

Нода должна появиться:

triangels-exit-fi-02
tag:exit
online


12. Step 8 — Configure NAT
Определяем сетевой интерфейс:

ip a

Пример:

ens3

Добавляем NAT:

iptables -t nat -A POSTROUTING -o ens3 -j MASQUERADE


13. Step 9 — Make NAT Persistent
Устанавливаем:

apt install iptables-persistent -y

Сохраняем правила.

14. Step 10 — Verify Exit Node
Проверяем статус:

tailscale status

Ожидаем:

offers exit node


15. Client Test
На клиентском устройстве:

tailscale up --exit-node=triangels-exit-fi-02

Проверяем внешний IP:

curl ifconfig.me

Ожидаемый результат:

PUBLIC_IP_EXIT_NODE


16. Health Check
Проверка соединения:

tailscale netcheck

Ожидается:

UDP: true
Nearest DERP: nearest location


17. Troubleshooting
Node not visible
Проверить:

headscale nodes list


NAT not working
Проверить:

iptables -t nat -L -n


Exit Node not offered
Проверить:

tailscale status


18. Security Model
Exit Nodes используют:
tag-based ACL
autoApprovers
Zero-Trust mesh routing
Все exit nodes получают тег:

tag:exit

ACL управляет доступом.

19. Scaling Strategy
TriAngels поддерживает масштабирование:

Multi Exit Layer
Geo Exit Routing
Federated Core Nodes

Пример:

EU Exit
US Exit
Asia Exit


20. Future Architecture
TriAngels будет поддерживать:

Federated Mesh
Partner Core Nodes
Multi-Region Control Planes


Document Owner
TriAngels Infrastructure Team
