# 🔒 Test-Projekt: HomeLab mit VLAN-Segmentierung

### Übersicht
Implementierung einer sicheren Netzwerkarchitektur durch Segmentierung verschiedener Geräteklassen mittels VLANs.
Ziel war die Erhöhung der Netzwerksicherheit und Isolation potenziell kompromittierter IoT-Geräte.

### Netzwerk-Topologie

![Netzwerk Diagramm](https://raw.githubusercontent.com/b00tkit/HomeLab_test/refs/heads/main/network_test.png)


### Technologie-Stack
- **Firewall/Router:** pfSense 2.7
- **Switch:** Managed Switch mit 802.1Q VLAN-Support
- **Protokolle:** Inter-VLAN Routing, DHCP, DNS
- **Security:** Firewall Rules, DHCP Snooping

### VLAN-Struktur

| VLAN ID | Name | Subnetz | Zweck |
|---------|------|---------|-------|
| 10 | Management | 192.168.10.0/24 | Admin-Zugriff |
| 20 | Workstations | 192.168.20.0/24 | Laptops, PCs |
| 30 | IoT | 192.168.30.0/24 | Smart Home Geräte |
| 40 | Guest | 192.168.40.0/24 | Gäste-WiFi |

### Security-Implementierung

**Firewall-Regeln:**
- Default Deny zwischen allen VLANs
- IoT-VLAN: Nur Internet-Zugriff, kein LAN
- Guest-VLAN: Komplett isoliert
- Management-VLAN: Nur von Admin-Geräten erreichbar

**Weitere Maßnahmen:**
- DHCP Snooping aktiviert
- Port Security auf Switch-Ebene
- Logging aller Deny-Rules
- Regelmäßige Firewall-Log-Analyse

### Herausforderung & Lösung

**Problem:**  
Nach VLAN-Trennung konnten Smart-Home-Geräte (VLAN 30) den Steuerungs-Hub (VLAN 20) nicht mehr erreichen.

**Troubleshooting:**
1. Packet Capture zwischen VLANs → mDNS-Traffic wurde geblockt
2. Firewall-Logs zeigten Deny auf Port 5353/UDP
3. Recherche: mDNS benötigt Multicast-Adresse 224.0.0.251

**Lösung:**
- mDNS Repeater auf pfSense konfiguriert
- Firewall-Regel: VLAN 30 → VLAN 20, Port 5353/UDP, nur zu Hub-IP
- Test mit Wireshark → Kommunikation erfolgreich

### Erkenntnisse

✅ Planung vor Implementierung spart Troubleshooting-Zeit  
✅ Logging ist essentiell für Fehleranalyse  
✅ Dokumentation während der Arbeit, nicht nachträglich  
✅ Schrittweise Migration vermeidet komplette Ausfälle

### Nächste Schritte
- [ ] IDS/IPS mit Suricata integrieren
- [ ] VPN für sicheren Remote-Zugriff
- [ ] Automatisierte Firewall-Backups
- [ ] SIEM-Integration für Log-Analyse


## 📚 Weitere Projekte

*In Planung:*
- SIEM Lab mit Wazuh
- Network Automation mit Python/Netmiko
- Site-to-Site VPN zwischen zwei Standorten
