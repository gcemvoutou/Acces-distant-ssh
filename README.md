# Accès à distance par SSH — TP BTS SIO SISR

> Mise en place et sécurisation d'un accès à distance par **SSH** sur un **Raspberry Pi 4** (serveur, sous Debian 13 « trixie »), depuis un **poste client Windows**.
> Ce dépôt documente l'intégralité du TP : du durcissement des mots de passe jusqu'aux tunnels SSH, en passant par l'authentification par clé et le transfert de fichiers.

---

## Objectif du TP

SSH (*Secure SHell*) est un protocole qui établit une liaison **chiffrée** entre un client et un serveur. Il remplace les anciens protocoles en clair comme Telnet. L'objectif de ce TP est de :

1. comprendre pourquoi un mot de passe faible est un risque (démonstration par cassage de hash) ;
2. mettre en place un serveur SSH et le configurer ;
3. s'y connecter par mot de passe, puis par **clé asymétrique** (plus sûr) ;
4. transférer des fichiers de façon chiffrée (`scp`, `sftp`) ;
5. se connecter sans retaper la passphrase grâce à l'**agent SSH** ;
6. créer des **tunnels SSH** pour chiffrer d'autres services (web, bureau graphique VNC).

---

## Contexte technique

| Élément | Détail |
|---|---|
| **Serveur** | Raspberry Pi 4 Model B (8 Go), Debian 13 « trixie », hostname `clararaspberry` |
| **Client** | PC Windows (`DESKTOP-1QLEJ6F`), OpenSSH intégré + PowerShell |
| **Utilisateur** | `clara` (sur le serveur comme sur le client) |
| **Réseau (salle)** | serveur en `192.168.0.206`, client en `192.168.0.100` |
| **Serveur SSH** | OpenSSH (`openssh-server`), port 22 |
| **Serveur web (tunnel)** | Apache2 (page par défaut Debian), port 80 |
| **Serveur VNC (tunnel)** | wayvnc, port 5900 |

> **Note.** Les adresses IP qui apparaissent dans les captures (`192.168.0.x`, `192.168.1.x`, `192.168.100.x`) sont des **adresses de réseau local privé** (lab), sans valeur en dehors du réseau concerné.

---

## Sommaire

| # | Partie | Contenu |
|---|---|---|
| I | [Mots de passe & cassage John](Partie-I-Mots-de-passe-et-cassage-John/mots-de-passe-john.md) | Création d'un utilisateur, hash `/etc/shadow`, cassage avec John the Ripper + rockyou |
| II | [Système de clés SSH](Partie-II-Systeme-de-cles-SSH/systeme-cles-ssh.md) | Chiffrement symétrique vs asymétrique, établissement d'une connexion SSH |
| III | [Installation & configuration](Partie-III-Installation-et-configuration/installation-config.md) | `openssh-server`, fichier `sshd_config`, options à décommenter |
| IV | [Se connecter en SSH](Partie-IV-Se-connecter-en-SSH/connexion-ssh.md) | Par mot de passe, puis par clé (génération, envoi, vérification, test) |
| V | [Transfert de fichiers](Partie-V-Transfert-de-fichiers/transfert-fichiers.md) | `scp` (envoi/réception), `sftp` (session interactive) |
| VI | [Se connecter sans mot de passe](Partie-VI-Se-connecter-sans-mot-de-passe/agent-ssh.md) | Agent SSH, `ssh-add` et ses options |
| VII | [Tunnels SSH](Partie-VII-Tunnels-SSH/tunnels-ssh.md) | Tunnel web (Apache) + bonus : bureau graphique via VNC |
| VIII | [Ne plus utiliser Telnet](Partie-VIII-Ne-plus-utiliser-Telnet/telnet.md) | Pourquoi Telnet est à proscrire |

---

## Structure du dépôt

```
.
├── README.md                              ← sommaire + contexte
├── Partie-I-Mots-de-passe-et-cassage-John/
│   ├── mots-de-passe-john.md
│   └── images/
├── Partie-II-Systeme-de-cles-SSH/
│   ├── systeme-cles-ssh.md
│   └── images/
├── Partie-III-Installation-et-configuration/
│   ├── installation-config.md
│   └── images/
├── Partie-IV-Se-connecter-en-SSH/
│   ├── connexion-ssh.md
│   └── images/
├── Partie-V-Transfert-de-fichiers/
│   ├── transfert-fichiers.md
│   └── images/
├── Partie-VI-Se-connecter-sans-mot-de-passe/
│   ├── agent-ssh.md
│   └── images/
├── Partie-VII-Tunnels-SSH/
│   ├── tunnels-ssh.md
│   └── images/
└── Partie-VIII-Ne-plus-utiliser-Telnet/
    └── telnet.md
```

---

## Compétences mises en œuvre

- Administration Linux (Debian / Raspberry Pi OS) en ligne de commande
- Configuration d'un service réseau (OpenSSH) et durcissement
- Cryptographie appliquée : chiffrement symétrique / asymétrique, couple de clés RSA
- Transfert de fichiers sécurisé (`scp`, `sftp`)
- Redirection de port / tunneling SSH (`-L`)
- Diagnostic réseau (`ip a`, `ping`, `ss`, résolution mDNS `.local`)
- Sécurité offensive / défensive : démonstration de cassage de mot de passe (John the Ripper)

---

*TP réalisé dans le cadre du BTS SIO option SISR — Clara EMVOUTOU.*
