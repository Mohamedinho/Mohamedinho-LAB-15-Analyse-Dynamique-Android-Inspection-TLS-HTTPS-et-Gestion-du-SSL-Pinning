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

<img width="849" height="1852" alt="image" src="https://github.com/user-attachments/assets/f26cc9c0-5879-4f43-953a-b8e5e288bc92" />

## 9. Téléchargement du certificat CA Burp

Depuis le navigateur de l’émulateur Android, l’adresse suivante a été ouverte :

http://burp

<img width="365" height="169" alt="image" src="https://github.com/user-attachments/assets/32305954-6032-4d38-aea5-2806566482cc" />

Cette page permet de télécharger le certificat CA généré par Burp Suite.

Fichier téléchargé :

## 10. Installation du certificat CA

Le certificat CA a ensuite été installé depuis les paramètres de sécurité Android.

Chemin utilisé :

Settings > Security > Encryption & credentials > Install a certificate > CA certificate
cacert.der

<img width="407" height="298" alt="image" src="https://github.com/user-attachments/assets/9613b3c9-2e85-4de4-a9cd-3a2f873613ce" />

Après l’installation, Android affiche le message suivant :

CA certificate installed

Cette étape permet à l’émulateur Android de faire confiance au certificat utilisé par Burp Suite pour intercepter le trafic HTTPS.

## 11. Lancement du serveur InsecureBankv2

L’application InsecureBankv2 nécessite un serveur backend local appelé AndroLabServer.


## 12. Configuration du serveur dans InsecureBankv2

Dans l’application InsecureBankv2, l’adresse du serveur backend a été configurée manuellement.

Configuration utilisée :

Server IP: 10.0.2.2
Server Port: 8080

<img width="899" height="1750" alt="image" src="https://github.com/user-attachments/assets/de253e97-3206-40c4-9b95-5a7b830f7863" />

Cette configuration permet à l’application Android de communiquer avec le backend lancé sur la machine Windows.


## 13. Connexion à l’application

Après la configuration du serveur, une tentative de connexion a été effectuée depuis l’application InsecureBankv2.

Après une connexion réussie, l’application affiche l’écran principal contenant les options suivantes :

<img width="393" height="470" alt="image" src="https://github.com/user-attachments/assets/a65ef7e2-ac5e-4bee-a399-b9ca825faf8c" />

Transfer
View Statement
Change Password
Device not Rooted!!

Cela confirme que l’application communique correctement avec le serveur backend.

## 14. Script de bypass SSL pinning avec Frida

Un script Frida a ensuite été utilisé afin d’installer plusieurs hooks liés à la gestion TLS/SSL.

Fichier utilisé :

sslpin_bypass_universal.js

Commande utilisée :

frida -U -f com.android.insecurebankv2 -l sslpin_bypass_universal.js

<img width="1751" height="898" alt="image" src="https://github.com/user-attachments/assets/a4ff3c39-3640-4463-bec5-13f80c352baf" />

Résultat obtenu :

[+] Universal SSL Pinning Bypass chargé
[+] Cible : com.android.insecurebankv2
[+] SSL bypass: SSLContext.init patché
[+] SSL bypass: TrustManagerImpl.verifyChain patché
[-] OkHttp CertificatePinner non trouvé
[+] SSL bypass: WebViewClient.onReceivedSslError patché
[+] Hooks SSL installés

Le message suivant n’est pas une erreur bloquante :

OkHttp CertificatePinner non trouvé

Cela signifie simplement que l’application ne semble pas utiliser la classe okhttp3.CertificatePinner. Les autres hooks SSL ont bien été chargés.

## 15. Observation du trafic dans Burp Suite

Après la configuration du proxy et l’utilisation de l’application, Burp Suite affiche les requêtes envoyées par InsecureBankv2 vers le serveur local.

Dans Burp Suite :

Proxy > HTTP history

<img width="1872" height="385" alt="image" src="https://github.com/user-attachments/assets/9182b9e1-9b23-4ec2-837b-9f3827f43e15" />

## 16. Détails de la requête login

Dans l’onglet HTTP history de Burp Suite, la requête /login peut être inspectée en détail.

<img width="1133" height="249" alt="image" src="https://github.com/user-attachments/assets/b8f8c39c-fd3f-4cca-aafe-876823e9eafc" />

## 17. Résultat obtenu

À la fin du lab, les éléments suivants ont été validés :

Frida détecte correctement l’application InsecureBankv2.
Un script Frida simple peut être injecté dans l’application.
Burp Suite est configuré comme proxy sur le port 8080.
L’émulateur Android utilise Burp Suite comme proxy.
Le certificat CA de Burp Suite est installé sur Android.
Le serveur backend InsecureBankv2 fonctionne sur le port 8888.
L’application communique correctement avec le serveur backend.
Les requêtes réseau sont visibles dans Burp Suite.
Le script Frida de bypass SSL pinning est chargé correctement.

## 18. Conclusion

Ce lab a permis de réaliser une analyse dynamique Android complète sur l’application InsecureBankv2.

Les principales étapes réalisées sont :

Détection de l’application avec Frida.
Injection d’un script Frida simple.
Configuration de Burp Suite comme proxy.
Installation du certificat CA Burp sur Android.
Lancement du serveur backend InsecureBankv2.
Configuration de l’application avec l’adresse IP du serveur.
Connexion à l’application.
Observation des requêtes réseau dans Burp Suite.
Injection d’un script de bypass SSL pinning avec Frida.

Ce lab montre comment combiner Frida et Burp Suite pour analyser le comportement réseau d’une application Android dans un environnement de test contrôlé.
