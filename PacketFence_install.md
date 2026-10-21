#### Prerequisites:

|          | Minimum                                                              | Recommended | Why                                                             |
| -------- | -------------------------------------------------------------------- | ----------- | --------------------------------------------------------------- |
| CPU      | 2 cores                                                              | 8 cores     | Same service load as on-prem.                                   |
| RAM      | 8192 MiB                                                             | 16384 MiB   | Gives headroom for MariaDB, haproxy, RADIUS and Fingerbank.     |
| Storage  | 40 GB                                                                | 200 GB      | The database is I/O-sensitive, and you can grow the disk later. |
| Net0     | Management + RADIUS, private IP only                                 |             | Access Admin UI on :1443 and the RADIUS listener.               |
| Net1/vpn | Site-to-site tunnel (WireGuard or IPsec) to the school/customer site |             | This is how the APs and switches reach RADIUS.                  |

---

#### Installation

**Step 1, Interfaces & Networks:** eth0 is the management interface (10.152.0.x, gateway 10.152.0.1, hostname `packetfence`, DNS 1.1.1.1). Click _New VLAN_ here if you want registration/isolation VLANs on the same interface. Use a static IP, since your switches and APs will point RADIUS at it.
![[PacketFence-1792583868257.webp|600]]

---

**Step 2, Configure PacketFence:**
- Set the **timezone** to `Europe/Brussels`. Left empty it follows the server, which is fine but explicit is cleaner.
- Leave _anonymous stats_ off.
- **Track configuration** shows a red status in your screenshot. It tracks everything under `/usr/local/pf/conf` in git, including passwords, so restrict access to the server.
- **Alerting:** the recipient `pf@localhost` is just a placeholder. Set a real address or you won’t see rogue DHCP or violation alerts.
- **Administrator password:** use the generate button and copy it into your password manager before continuing.

![[PacketFence-1792583928992.webp|600]]

---

**Step 3, Fingerbank:** optional. Registering gives you an API key that improves device fingerprinting (device type and OS detection). Worth doing for a NAC lab, otherwise skip.
![[PacketFence-1792584058656.webp|600]]

---

**Step 4, Confirmation:** this screen shows three credential sets:
- Database root account (`root`)
- Database user account (`pf`)
- Administrator account (`admin`)

![[PacketFence-1792584432049.webp|600]]
