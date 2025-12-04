# Incident - Interface graphique active sur serveur de production

## Infos de base

**Date :** 4 décembre 2025  
**Serveur :** srv-prod-01  
**Gravité :** Élevée  
**Statut :** Résolu  

---

## Ce qui s'est passé

Aujourd'hui pendant mes vérifications quotidiennes (c'est mon premier jour en tant que sysadmin junior), j'ai remarqué un truc bizarre en checkant la RAM avec `free -h`.

Le serveur utilisait pas mal de swap (67 MB) alors qu'il y avait encore de la RAM dispo. J'ai voulu voir ce qui consommait autant avec :
```bash
ps aux --sort=-%mem | head -10
```

Et là, surprise : GNOME tourne sur le serveur ! 

Les processus les plus gourmands :
- packagekitd : 622 MB
- gnome-shell : 326 MB  
- plein d'autres trucs graphiques (gnome-software, Xwayland, etc.)

---

## Vérifications

J'ai vérifié le target par défaut : `systemctl get-default`

Résultat : `graphical.target`

**Problèmes identifiés :**
- Environ 1 GB de RAM gaspillée
- Risque de sécurité accru
- Mauvaise pratique pour un serveur de prod

---

## Solution

Changement en mode texte :
```bash
systemctl set-default multi-user.target
```

---

## Résultat

RAM libérée : ~1 GB
Services normaux uniquement (firewalld, php-fpm, etc.)

---

## Ce que j'ai appris

1. Toujours vérifier la config de base
2. Les serveurs de prod = mode texte uniquement
3. Le swap peut indiquer un problème de config
