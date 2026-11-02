# TODO-PREUVE — Configuration FortiGate réelle

**Préalable :** aucune VM FortiGate n'est présente dans l'inventaire VirtualBox actuel
(Ubuntu-Lab, Kali-Lab, pfSense-lab, DC, Windows-LTCS-lab). Pour produire les preuves,
déployer une VM FortiGate (image d'évaluation FortiOS) ou utiliser un équipement réel.

**Manip :**
1. Créer les 4 zones/interfaces et l'adressage de `topology/README.md`.
2. Implémenter les règles de `policies/matrice_flux.md` (objets, groupes, politiques).
3. Activer la journalisation du deny implicite.
4. Tester quelques flux : un autorisé (LAN→DMZ:443) et un refusé (DMZ→LAN) pour prouver
   la segmentation.
5. Exporter la config : `show` (CLI) ou sauvegarde depuis l'interface.

**Anonymisation OBLIGATOIRE avant commit :**
- Masquer IP publiques, mots de passe admin, `psksecret` VPN, certificats, clés API.
- Déposer la config nettoyée dans `policies/` (ex. `fortigate-export.sanitized.conf`).

**Fichiers attendus :**
- `policies/fortigate-export.sanitized.conf` : config réelle anonymisée.
- `captures/policies_list.png` : liste des politiques dans l'interface.
- `captures/test_flux_autorise.png` et `captures/test_flux_refuse.png`.
- `captures/log_deny_implicite.png` : journal d'un flux refusé.
