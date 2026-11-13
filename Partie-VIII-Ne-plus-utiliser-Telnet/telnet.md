# Partie-VIII-Ne-plus-utiliser-Telnet

> ⬅️ [Partie précédente](../Partie-VII-Tunnels-SSH/tunnels-ssh.md) · [Retour au sommaire](../README.md)

**Telnet** transmet **toutes les données en clair**, y compris l'identifiant et le mot de passe. N'importe qui sur le réseau peut donc les **intercepter** (par exemple avec un analyseur de trames comme Wireshark).

**SSH le remplace** car il **chiffre toute la communication** : identifiants, commandes et données transférées.

> 🔒 **Règle à retenir.** Telnet ne doit être utilisé que sur du **vieux matériel** qui ne supporte pas SSH (certains équipements réseau anciens). Partout ailleurs, on utilise **SSH**.

### Comparatif

| | Telnet | SSH |
|---|---|---|
| Chiffrement | ❌ aucun (tout en clair) | ✅ tout chiffré |
| Port par défaut | 23 | 22 |
| Mot de passe | visible sur le réseau | protégé |
| Authentification par clé | ❌ | ✅ |
| Usage recommandé | matériel ancien uniquement | par défaut |

---

> ⬅️ [Partie précédente](../Partie-VII-Tunnels-SSH/tunnels-ssh.md) · [Retour au sommaire](../README.md)
