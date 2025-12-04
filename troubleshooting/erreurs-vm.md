# 🔧 Erreurs communes - Machines Virtuelles

## piix4_smbus

**Erreur :**
```
piix4_smbus 0000:00:07.3: SMBus Host Controller not enabled!
```

**Cause :** Driver VM (VirtualBox/VMware)  
**Gravité :** ⚪ Bénin  
**Action :** Ignorer

---

## alsactl

**Erreur :**
```
alsactl: failed to import hw:0
```

**Cause :** Pas de carte son  
**Gravité :** ⚪ Bénin  
**Action :** Ignorer

---

## Interface graphique sur serveur

**Erreur :** Processus GNOME/GDM actifs  
**Gravité :** 🔴 Critique  
**Action :**
```bash
systemctl set-default multi-user.target
```

**Référence :** Voir incidents/2025-12-04-gui-production.md

---

*Dernière mise à jour : 4 décembre 2025*
