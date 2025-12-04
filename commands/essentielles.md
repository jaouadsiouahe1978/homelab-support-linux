# 🛠️ Commandes essentielles

## 📊 Monitoring
```bash
# Espace disque
df -h
du -sh /* | sort -h

# Mémoire
free -h
ps aux --sort=-%mem | head -10

# Charge
uptime
top
```

## 📋 Logs
```bash
# Erreurs du jour
journalctl -p err --since today

# Suivre en temps réel
journalctl -f

# Par service
journalctl -u nginx.service

# Nettoyer vieux logs
journalctl --vacuum-time=30d
```

## 🔧 Services
```bash
# Statut
systemctl status nom

# Démarrer/Arrêter
systemctl start nom
systemctl stop nom
systemctl restart nom

# Services en échec
systemctl --failed
```

## 🌐 Réseau
```bash
# Adresses IP
ip a

# Ports en écoute
ss -tulpn

# Test connectivité
ping google.com
curl -I https://exemple.com
```

## 👤 Utilisateurs
```bash
# Qui est connecté
w
who

# Historique
last
```

---
*Dernière mise à jour : 4 décembre 2025*
