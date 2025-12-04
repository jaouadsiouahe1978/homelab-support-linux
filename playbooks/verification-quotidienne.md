# 📋 Playbook - Vérification quotidienne des serveurs

## Objectif
Contrôle de santé quotidien de l'infrastructure pour détecter les problèmes avant qu'ils deviennent critiques.

## Pré-requis
- Accès SSH aux serveurs
- Droits sudo
- Terminal ouvert

## Durée estimée
15-20 minutes par serveur

---

## 1️⃣ Vérification espace disque

### Commande
```bash
df -h
```

### Seuils d'alerte
- ⚠️ 80-89% : Surveillance renforcée
- 🔴 90%+ : Action immédiate requise

### Actions si > 80%
```bash
# Trouver les gros répertoires
du -sh /* | sort -h

# Nettoyer les logs si nécessaire
journalctl --vacuum-time=30d

# Analyser /var/log
du -sh /var/log/*
```

---

## 2️⃣ Vérification RAM et Swap

### Commande
```bash
free -h
```

### Indicateurs normaux
- Swap utilisé < 100 MB : ✅ OK
- Swap 100-500 MB : ⚠️ Surveiller
- Swap > 500 MB : 🔴 Investiguer

### Si swap élevé
```bash
# Identifier les processus gourmands
ps aux --sort=-%mem | head -10

# Vérifier charge système
uptime
```

---

## 3️⃣ Analyse des logs d'erreur

### Commande
```bash
sudo journalctl -p err --since today
```

### Erreurs à ignorer (machines virtuelles)
- `piix4_smbus` - Driver VM
- `alsactl` - Carte son absente

### Erreurs critiques nécessitant action
- Tout ce qui contient : `failed`, `critical`, `oom`, `denied`

---

## 4️⃣ Services critiques

### Vérifier le statut
```bash
systemctl status nginx
systemctl status postgresql
systemctl status php-fpm
```

### Vérifier les services en échec
```bash
systemctl --failed
```

---

## 5️⃣ Charge système

### Commande
```bash
uptime
```

### Interprétation du load average
Format : `load average: 0.52, 0.58, 0.59` (1min, 5min, 15min)

- Load < nombre de CPUs : ✅ Normal
- Load = nombre de CPUs : ⚠️ Limite
- Load > nombre de CPUs : 🔴 Surcharge

---

## 6️⃣ Vérification target système

### Commande
```bash
systemctl get-default
```

### Valeur attendue
✅ `multi-user.target` (mode texte)  
❌ `graphical.target` (interface graphique - incident!)

---

## ✅ Checklist résumée

- [ ] Espace disque < 80%
- [ ] Swap < 100 MB
- [ ] Aucune erreur critique dans les logs
- [ ] Tous les services critiques UP
- [ ] Load average normal
- [ ] Target = multi-user

---

*Créé le : 4 décembre 2025*
