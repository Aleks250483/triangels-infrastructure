# 📘 TriAngels Emergency Card (самое главное)

> Назначение: быстрые действия, если пропала связь или случилась авария на VPS/WireGuard.

---

## 🚨 ЕСЛИ VPN ПРОПАЛ

### 1️⃣ Проверить состояние WireGuard
```bash
sudo wg

Если нет handshake → дальше.

2️⃣ Проверить firewall
sudo ufw status

Должно быть:

51820/udp ALLOW

3️⃣ Перезапустить WireGuard
sudo systemctl restart wg-quick@wg0
4️⃣ Восстановить GOLD (если всё плохо)
sudo gold-restore

✅ 90% проблем решено.

🚨 ЕСЛИ VPS УМЕР ПОЛНОСТЬЮ (Disaster Recovery)

Новый VPS →

apt update && apt install wireguard ufw -y

Потом:

scp GOLD backup
sudo gold-restore

🔥 Сеть возвращается.

🚨 ПРОВЕРКА: ЖИВА ЛИ СЕТЬ
VPS:
sudo wg
NAS:
docker exec -it wireguard ping 10.81.0.1
