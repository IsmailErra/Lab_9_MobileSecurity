# Lab_9_MobileSecurity

## Objectif
Examiner rapidement une application Android vulnérable en utilisant **Drozer**.

---

## Prérequis
- Android Studio avec un émulateur.
- `adb` installé.
- Drozer (`pip install drozer`).
- Fichiers `drozer-agent.apk` et `VulnerableApp.apk` dans ce dossier.

---

## Étape 1 – Installation
1. Démarrez un émulateur via *AVD Manager*.
2. Installez l'agent :
   ```bash
   adb install drozer-agent.apk
   ```
3. Installez l'application vulnérable :
   ```bash
   adb install VulnerableApp.apk
   ```
4. Ouvrez **Drozer Agent**, activez *Embedded Server*.
5. Configurez le port :
   ```bash
   adb forward tcp:31415 tcp:31415
   ```
> Vérifiez que tout est en place.

![Émulateur avec Drozer activé](screenshots/Screenshot%202026-05-25%20163845.png)

---

## Étape 2 – Connexion depuis le PC
```bash
drozer console connect
```
- Vérifiez la connexion :`dz> device`
- Listez les modules :`dz> list`
- Affichez la version Android :`dz> run information.device`

![Console Drozer connectée](screenshots/Screenshot%202026-05-25%20163903.png)

---

## Étape 3 – Cartographie des composants exposés
```bash
dz> run app.package.list
```
- Identifiez `com.example.vulnerableapp`.
- Listez activités, services, receivers, providers :
  ```bash
dz> run app.activity.info -a com.example.vulnerableapp
  dz> run app.service.info -a com.example.vulnerableapp
  dz> run app.broadcast.info -a com.example.vulnerableapp
  dz> run app.provider.info -a com.example.vulnerableapp
  ```
- Résumez dans le tableau :

| Type      | Nom                 | Exporté | Protection |
|-----------|--------------------|---------|------------|
| Activity  | LoginActivity      | Oui     | Aucun      |
| Activity  | UserProfileActivity| Oui     | Permission |
| Service   | DataSyncService    | Oui     | Aucun      |
| Receiver  | BootReceiver       | Oui     | Aucun      |
| Provider  | UserDataProvider   | Oui     | Lecture/Écriture |

![Cartographie des composants](screenshots/Screenshot%202026-05-25%20163914.png)

---

## Étape 4 – Vérifier les protections
- Affichez le manifeste :`dz> run app.package.manifest com.example.vulnerableapp`
- Vérifiez les `intent‑filters` et les attributs `exported`.
- Pour les providers, contrôlez les permissions :`dz> run app.provider.info -a com.example.vulnerableapp -p`
- Testez l’accès sans permission :`dz> run scanner.provider.finduris -a com.example.vulnerableapp`

![Analyse des protections](screenshots/Screenshot%202026-05-25%20163931.png)

---

## Étape 5 – Rassembler les preuves
Créez la structure :
```
preuves/
  activities/
  services/
  receivers/
  providers/
  manifest/
```
Enregistrez les listes, les tableaux et les captures d’écran.

---

## Livrables
1. Ce README (simplifié).
2. Le dossier **preuves** complet.
3. Un bref rapport des vulnérabilités.

---

*Suivez les bonnes pratiques de documentation et de sécurité.*
