# Mohamedinho-LAB-15-Analyse-Dynamique-Android-Inspection-TLS-HTTPS-et-Gestion-du-SSL-Pinning

## 1. Objectif du lab

Ce laboratoire a pour but de réaliser une analyse dynamique d’une application Android volontairement vulnérable afin d’étudier son trafic réseau et de tester l’injection de scripts à l’aide de Frida.

L’application utilisée dans ce lab est **InsecureBankv2**, une application Android conçue spécialement pour l’apprentissage et la pratique de la sécurité mobile.

Les objectifs principaux de ce lab sont les suivants :

- Vérifier la communication entre Frida et l’émulateur Android.
- Identifier l’application Android cible avec Frida.
- Injecter un script Frida simple dans l’application.
- Configurer Burp Suite comme proxy d’interception.
- Installer le certificat CA de Burp Suite sur l’émulateur Android.
- Configurer le proxy réseau sur Android.
- Lancer le serveur backend de InsecureBankv2.
- Connecter l’application Android au serveur local.
- Observer les requêtes HTTP dans Burp Suite.
- Tester un script Frida de bypass SSL pinning.

---

## 2. Environnement utilisé

Le lab a été réalisé avec l’environnement suivant :

- Windows
- PowerShell
- Android Studio Emulator
- ADB
- Frida 17.8.0
- frida-server
- Burp Suite Community Edition
- Python 2.7
- InsecureBankv2
- Backend AndroLabServer

---

## 3. Application cible

L’application utilisée pour ce lab est :

**InsecureBankv2**

## 4. Structure du projet
LAB15-SSL-Pinning-Frida/
│
├── README.md
├── hello.js
├── sslpin_bypass_universal.js

## 5. Vérification de Frida et détection de l’application

La première étape consiste à vérifier que Frida peut communiquer correctement avec l’émulateur Android et détecter les applications installées.

Commande utilisée :

frida-ps -Uai

<img width="1100" height="531" alt="Capture d&#39;écran 2026-05-06 143544" src="https://github.com/user-attachments/assets/04e3d59a-95be-4068-b7d0-0b37145eadf0" />
