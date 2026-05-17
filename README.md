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

Résultat obtenu :

PID   Name            Identifier

## 6. Test d’injection simple avec Frida

Avant d’utiliser un script de bypass SSL pinning, un script simple a été injecté afin de vérifier que Frida fonctionne correctement avec l’application.
7288  InsecureBankv2  com.android.insecurebankv2

Fichier utilisé :

hello.js

Contenu du script :

<img width="1196" height="417" alt="588347384-fb05f33a-d898-46e2-9246-fec364711f1b" src="https://github.com/user-attachments/assets/e7483658-11e6-430c-8bac-af653ec188cb" />

Commande utilisée :

frida -U -f com.android.insecurebankv2 -l hello.js

Résultat obtenu :

Connected to Android Emulator 5554
Spawned `com.android.insecurebankv2`. Resuming main thread!
[+] Script injecté: Java.perform OK

Cette étape confirme que Frida est capable de lancer l’application et d’injecter du code JavaScript dans son processus.

## 7. Configuration de Burp Suite

Burp Suite a été utilisé comme proxy afin d’intercepter le trafic réseau généré par l’application Android.

Dans Burp Suite, la configuration se fait depuis :

<img width="819" height="547" alt="image" src="https://github.com/user-attachments/assets/290084a0-44f0-43d6-a87d-81fbc762dace" />

Le choix All interfaces permet à Burp Suite d’écouter sur toutes les interfaces réseau de la machine, notamment l’adresse IP locale utilisée par l’émulateur Android.

## 8. Configuration du proxy Android

Dans l’émulateur Android, le réseau Wi-Fi a été configuré afin de rediriger le trafic vers Burp Suite.

Chemin utilisé :
Settings > Network & Internet > Wi-Fi > AndroidWifi > Edit

<img width="977" height="664" alt="Capture d&#39;écran 2026-05-06 143956" src="https://github.com/user-attachments/assets/2fa2fa69-d194-43d4-bb1a-8094635ed064" />

