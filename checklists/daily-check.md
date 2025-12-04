# ✅ Checklist quotidienne

**Serveur :** _____________  
**Date :** _____________  
**Sysadmin :** _____________

## Vérifications système

- [ ] `df -h` - Espace disque < 80%
- [ ] `free -h` - Swap < 100 MB
- [ ] `uptime` - Load normal
- [ ] `systemctl get-default` - multi-user.target
- [ ] `systemctl --failed` - Aucun service en échec

## Logs et erreurs

- [ ] `journalctl -p err --since today` - Pas d'erreurs critiques

## Services applicatifs

- [ ] nginx/apache - UP
- [ ] postgresql/mysql - UP  
- [ ] php-fpm - UP

## Notes du jour
```
[Observations]
```

## Actions requises
```
[Problèmes à suivre]
```

---
**Signature :** _____________
