# 🌐 Projet Réseau Multi-Sites - Paris, Lyon & Marseille

## 🧭 Présentation générale

Ce projet met en place un **réseau d’entreprise interconnecté** réparti sur **trois sites géographiques** :
- 🏙️ **Paris**
- 🏙️ **Lyon**
- 🏙️ **Marseille**

Chaque site possède ses **propres VLANs** (Utilisateurs, Wi-Fi, Imprimantes, DMZ...) et communique avec les autres via des **liaisons WAN série**.  
Le tout est géré à l’aide du **routage dynamique OSPF**, pour une interconnexion automatique et évolutive.

Le projet a été conçu et simulé sous **Cisco Packet Tracer**, avec un **adressage VLSM** optimisé pour une gestion efficace des IP.

---

## 🎯 Objectifs du projet

- Interconnecter les 3 sites (Paris, Lyon, Marseille) via des liaisons WAN.  
- Permettre la **communication inter-sites** entre tous les utilisateurs.  
- Assurer l’**accès aux imprimantes** depuis n’importe quel site.  
- Optimiser l’adressage avec le **VLSM (Variable Length Subnet Masking)**.  
- Implémenter le **routage dynamique OSPF**.  
- Séparer les réseaux locaux via des **VLANs** (sécurité et organisation).  
- Configurer les routeurs en **Router-on-a-Stick** avec des liens **trunk**.

---

## 🗺️ Topologie du réseau

    +-------------+
    |   PARIS     |
    | (Router4)   |
    +-------------+
          |
    10.10.10.0/30
          |
    +-------------+
    |    LYON     |
    | (Router5)   |
    +-------------+
          |
    10.10.10.4/30
          |
    +-------------+
    | MARSEILLE   |
    | (Router6)   |
    +-------------+

Chaque site est relié à son **switch local** en **trunk**, et les VLANs sont gérés sur les sous-interfaces du routeur.

---

## 🌐 Plan d’adressage complet (VLSM)

### 🔸 Réseaux Locaux (LAN)

| Site | VLAN | Nom | Réseau | Masque | Gateway |
|------|------|-----|---------|---------|----------|
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

### 🔸 Liaisons WAN (entre routeurs)

| Lien | Réseau | Masque | Routeur gauche | Routeur droit |
|------|---------|---------|----------------|----------------|
| Paris ↔ Lyon | 10.10.10.0/30 | 255.255.255.252 | 10.10.10.1 | 10.10.10.2 |
| Lyon ↔ Marseille | 10.10.10.4/30 | 255.255.255.252 | 10.10.10.5 | 10.10.10.6 |

---

## 🧱 VLANs par site

### 🏙️ Paris
| VLAN | Nom | Réseau |
|------|------|----------|
| 10 | USERS_PARIS | 10.0.0.0/20 |
| 20 | WIFI_PARIS | 10.0.16.0/24 |
| 30 | DMZ_PARIS | 10.0.17.0/26 |
| 40 | PRINTER_PARIS | 10.0.17.64/26 |

---

### 🏙️ Lyon
| VLAN | Nom | Réseau |
|------|------|----------|
| 50 | USERS_LYON | 10.0.17.128/21 |
| 60 | WIFI_LYON | 10.0.25.0/24 |
| 70 | PRINTER_LYON | 10.0.26.0/26 |

---

### 🏙️ Marseille
| VLAN | Nom | Réseau |
|------|------|----------|
| 80 | USERS_MARSEILLE | 10.0.26.64/26 |
| 90 | WIFI_MARSEILLE | 10.0.28.64/25 |
| 100 | PRINTER_MARSEILLE | 10.0.28.192/26 |

---
