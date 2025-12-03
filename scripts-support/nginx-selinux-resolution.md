# Nginx : Port bloqué par SELinux

> Résolution de l'erreur `bind() failed (13: Permission denied)`

---

## 🔴 Le Problème

Lors du démarrage de Nginx sur un port personnalisé (ex: 8081), le service échoue avec l'erreur suivante :
```
nginx: [emerg] bind() to 0.0.0.0:8081 failed (13: Permission denied)
```

---

## 🔍 Diagnostic

| Commande | Description |
|----------|-------------|
| `systemctl status nginx` | Vérifier l'état du service |
| `journalctl -xeu nginx` | Voir les logs détaillés |
| `getenforce` | Vérifier si SELinux est actif |
| `semanage port -l \| grep http` | Lister les ports HTTP autorisés |

---

## 💡 Explication

**SELinux** (Security-Enhanced Linux) applique des politiques de sécurité strictes. Par défaut, Nginx est autorisé uniquement sur les ports **80**, **443** et **8080**. Tout autre port est bloqué.

### Analogie 🏠

| Nginx dit : | SELinux répond : |
|-------------|------------------|
| *"Je veux ouvrir l'appartement 8081"* | *"Non ! Tu n'as accès qu'aux 80 et 443"* |

---

## ✅ Solution

### Étape 1 : Ajouter le port à SELinux
```bash
sudo semanage port -a -t http_port_t -p tcp 8081
```

| Option | Signification |
|--------|---------------|
| `-a` | Ajouter |
| `-t http_port_t` | Type HTTP |
| `-p tcp` | Protocole TCP |

### Étape 2 : Redémarrer Nginx
```bash
sudo systemctl restart nginx
```

### Étape 3 : Vérifier
```bash
ss -tlnp | grep nginx
```

---

## 📖 Comprendre `ss -tlnp`

| Option | Signification | Analogie 🏠 |
|--------|---------------|-------------|
| `-t` | TCP seulement | Seulement les appartements |
| `-l` | Listen (en écoute) | Ceux avec la lumière allumée |
| `-n` | Numérique | Numéro d'appart |
| `-p` | Process | Qui habite dedans ? |

---

## 🚨 Guide des erreurs

| Message | Cause probable |
|---------|----------------|
| `Connexion refusée` | Service arrêté |
| `403 Forbidden` | Permissions / SELinux |
| `502 Bad Gateway` | PHP-FPM KO |
| `Connection timed out` | Firewall |

---

*Environnement : Oracle Linux / RHEL / CentOS avec SELinux Enforcing*
