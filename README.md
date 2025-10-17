# 🌐 Projet Réseau Multi-Sites - Paris, Lyon, Marseille

## 📋 Description du projet

Ce projet met en place une **infrastructure réseau interconnectée** entre trois villes : **Paris**, **Lyon** et **Marseille**.  
Chaque site possède ses propres VLANs (utilisateurs, Wi-Fi, imprimantes) et les trois sites communiquent entre eux via des **liaisons WAN série**.

Le projet a été conçu et simulé sous **Cisco Packet Tracer** avec une configuration **VLSM**, un **routage OSPF** et une architecture modulaire.

---

## 🗺️ Objectifs

- Interconnecter les trois sites de l’entreprise.  
- Permettre la communication entre tous les utilisateurs, quelle que soit leur localisation.  
- Autoriser l’impression inter-sites (depuis n’importe quelle ville vers une imprimante d’une autre).  
- Optimiser l’adressage IP avec le **VLSM**.  
- Utiliser un **routage dynamique OSPF** pour simplifier l’administration.

---

## 🧱 Architecture générale


---

## 🧩 Plan d’adressage global

| Site | VLAN | Fonction | Réseau | Masque | Gateway |
|------|------|-----------|---------|---------|----------|
| **Paris** | 10 | Users | 10.0.0.0/20 | 255.255.240.0 | 10.0.0.1 |
|  | 20 | WiFi | 10.0.16.0/24 | 255.255.255.0 | 10.0.16.1 |
|  | 30 | DMZ | 10.0.17.0/26 | 255.255.255.192 | 10.0.17.1 |
|  | 40 | Printer | 10.0.17.64/26 | 255.255.255.192 | 10.0.17.65 |
| **Lyon** | 50 | Users | 10.0.17.128/21 | 255.255.248.0 | 10.0.17.129 |
|  | 60 | WiFi | 10.0.25.0/24 | 255.255.255.0 | 10.0.25.1 |
|  | 70 | Printer | 10.0.26.0/26 | 255.255.255.192 | 10.0.26.1 |
| **Marseille** | 80 | Users | 10.0.26.64/26 | 255.255.255.192 | 10.0.26.65 |
|  | 90 | WiFi | 10.0.28.64/25 | 255.255.255.128 | 10.0.28.65 |
|  | 100 | Printer | 10.0.28.192/26 | 255.255.255.192 | 10.0.28.193 |
| **WAN** | - | Paris ↔ Lyon | 10.10.10.0/30 | 255.255.255.252 | .1 / .2 |
| **WAN** | - | Lyon ↔ Marseille | 10.10.10.4/30 | 255.255.255.252 | .5 / .6 |

---

## ⚙️ Routage (OSPF)

Chaque routeur participe au **même domaine OSPF** :

```bash
router ospf 1
 network 10.0.0.0 0.255.255.255 area 0

