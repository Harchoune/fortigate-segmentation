# Matrice de flux inter-zones

Pièce maîtresse de la segmentation : chaque flux autorisé est justifié. Tout ce qui
n'est pas listé est **refusé par défaut** et journalisé.

Légende décision : ✅ autorisé · ⛔ refusé (implicite, rappelé pour les cas sensibles).

| # | Source | Destination | Service / Port | Décision | Justification sécurité |
|---|---|---|---|---|---|
| 1 | LAN | WAN (Internet) | HTTP/HTTPS (80/443), DNS (53) | ✅ | Navigation web des utilisateurs ; DNS vers le résolveur autorisé seulement. |
| 2 | LAN | DMZ | HTTPS (443) | ✅ | Accès des utilisateurs au site web interne/exposé, en HTTPS uniquement. |
| 3 | LAN | SERVERS | Applicatif (ex. 1433/SQL via appli, 445 si partage) | ✅ | Accès métier aux applications internes, ports applicatifs précis. |
| 4 | DMZ | SERVERS | Port applicatif back-end précis (ex. 5432) | ✅ | Le serveur web interroge la base ; un seul port, une seule destination. |
| 5 | DMZ | LAN | — | ⛔ | Un serveur exposé ne doit jamais initier de connexion vers le réseau utilisateurs. |
| 6 | DMZ | WAN | HTTPS (443) sortant (maj) | ✅ | Mises à jour sortantes du serveur, restreintes ; pas d'accès entrant initié par la DMZ ailleurs. |
| 7 | WAN | DMZ | HTTPS (443) | ✅ | Publication du service web (avec profil IPS/WAF si disponible). |
| 8 | WAN | LAN / SERVERS / ADMIN | — | ⛔ | Aucune connexion entrante directe depuis Internet vers l'interne. |
| 9 | ADMIN | LAN / DMZ / SERVERS | SSH (22), RDP (3389), HTTPS (443) gestion | ✅ | Administration hors-bande depuis la zone dédiée uniquement. |
| 10 | LAN / DMZ / SERVERS | ADMIN | — | ⛔ | La zone d'administration n'est jamais joignable depuis les zones de production. |
| 11 | SERVERS | WAN | HTTPS (443) maj, NTP (123) | ✅ | Mises à jour et synchro horaire sortantes, services restreints. |
| 12 | Toutes | Toutes (non listé) | any | ⛔ (journalisé) | Deny implicite par défaut, avec log pour détecter les flux anormaux. |

## Notes

- Les ports « applicatifs » exacts dépendent des services réellement déployés : les
  resserrer au strict nécessaire (un service, une destination).
- Préférer des objets d'adresse et des groupes de services nommés plutôt que des plages
  larges, pour que chaque règle reste lisible et auditable.
- Activer le NAT uniquement là où c'est requis (sortie WAN) ; pas de NAT inter-zones internes.
