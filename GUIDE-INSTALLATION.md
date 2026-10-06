# Déménagement SBA : installation sur votre propre site

Durée : environ 20 minutes, une seule fois. Gratuit (Firebase + Netlify, offres gratuites).

Contenu du dossier :

- `site/` : l'application, à mettre en ligne
- `firestore.rules` : règles de sécurité, à coller dans Firebase
- `sauvegarde_sba_2026-10-06.json` : vos données actuelles (3 missions + infos entreprise), à importer. **Ne le mettez pas en ligne.**

---

## Étape 1 : créer le projet Firebase (base de données + connexions)

1. Allez sur **https://console.firebase.google.com** et connectez-vous avec **issamemeallem@gmail.com**.
2. **Créer un projet**. Nom : `sba-demenagement`. Google Analytics : désactivé. Puis **Créer**.
3. Menu de gauche **Build → Authentication**, puis **Commencer**. Onglet **Sign-in method**, choisissez **E-mail/Mot de passe**, **Activer**, puis **Enregistrer**.
4. Toujours dans Authentication, onglet **Users**, cliquez **Ajouter un utilisateur** :
   - E-mail : `issamemeallem@gmail.com`
   - Mot de passe : choisissez-en un solide (c'est le compte administrateur).
5. Menu **Build → Firestore Database**, puis **Créer une base de données**. Emplacement : `eur3 (Europe)`. Choisissez **Mode production**, puis **Créer**.
6. Dans Firestore, ouvrez l'onglet **Règles**. Effacez tout, collez le contenu du fichier `firestore.rules`, puis **Publier**.

## Étape 2 : relier l'application à Firebase

1. Dans Firebase, cliquez sur l'icône ⚙️ puis **Paramètres du projet**. En bas, **Vos applications**, cliquez l'icône **`</>`** (Web).
2. Nom : `sba-web`, puis **Enregistrer l'application** (ne cochez pas Hosting).
3. Firebase affiche un bloc `const firebaseConfig = { apiKey: "...", ... }`.
4. Ouvrez `site/firebase-config.js` avec le Bloc-notes et remplacez chaque `COLLEZ_ICI_…` par la valeur correspondante. Enregistrez.

## Étape 3 : mettre le site en ligne (Netlify, glisser-déposer)

1. Allez sur **https://app.netlify.com/drop** et créez un compte gratuit.
2. **Glissez le dossier `site`** dans la page. Après quelques secondes, vous obtenez une adresse du type `https://xxxx.netlify.app`.
3. Facultatif : dans Netlify, **Site configuration → Change site name**, par exemple `sba-demenagement`, ce qui donne `https://sba-demenagement.netlify.app`.
4. **Important :** retournez dans Firebase, ouvrez **Authentication → Settings → Domaines autorisés → Ajouter un domaine** et collez `sba-demenagement.netlify.app` (votre adresse, sans `https://`).

> Pour modifier l'application plus tard, glissez à nouveau le dossier `site` dans Netlify (**Deploys → Drag and drop**).

## Étape 4 : première connexion (vous, l'administrateur)

1. Ouvrez votre adresse Netlify et connectez-vous avec `issamemeallem@gmail.com` et votre mot de passe.
2. L'application demande de **confirmer votre e-mail**. Cliquez **Envoyer l'e-mail de confirmation**, ouvrez votre Gmail, cliquez le lien, puis revenez et cliquez **J'ai confirmé, continuer**.
3. Cliquez **Entreprise**, puis **Importer une sauvegarde…** et choisissez `sauvegarde_sba_2026-10-06.json`. Vos 3 missions, vos frais, vos infos d'entreprise et la numérotation des factures (prochaine : 005) sont récupérés.

## Étape 5 : ajouter vos employés

1. Cliquez **Employés**.
2. Entrez le nom, l'e-mail, un mot de passe (6 caractères minimum) et le rôle :
   - **Employé** : ajoute et modifie les déménagements et transports, crée les factures. Il ne voit pas les bénéfices, l'analyse, les infos de l'entreprise, et ne peut rien supprimer.
   - **Administrateur** : accès complet, comme vous.
3. Envoyez à l'employé l'adresse du site, son e-mail et son mot de passe.
4. Pour bloquer un employé, cliquez **Désactiver** : l'accès est coupé tout de suite. **Nouveau mot de passe** lui envoie un e-mail de réinitialisation.

## Sur le téléphone (comme une application)

- **iPhone (Safari)** : ouvrez l'adresse, touchez Partager ⬆️, puis **Sur l'écran d'accueil**.
- **Android (Chrome)** : ouvrez l'adresse, touchez ⋮, puis **Ajouter à l'écran d'accueil**.

## Sécurité et sauvegardes

- Les données sont dans votre projet Firebase (Google), protégées par les règles : seuls les comptes que vous créez y ont accès.
- Les frais et bénéfices sont stockés à part et ne sont lisibles que par les administrateurs.
- Faites une sauvegarde chaque mois : **Entreprise**, puis **Sauvegarde complète (.json)**. Gardez le fichier sur votre PC ou Google Drive.

## Problèmes fréquents

| Message | Solution |
|---|---|
| « Configuration manquante » | `firebase-config.js` n'est pas rempli. Refaites l'étape 2, puis glissez à nouveau le dossier dans Netlify. |
| La connexion échoue sur le site mais l'e-mail est correct | Ajoutez le domaine Netlify dans **Domaines autorisés** (étape 3.4). |
| « Ce compte n'a pas accès » | L'employé n'a pas été créé depuis le panneau **Employés**. |
| « Accès refusé » en haut de l'écran | Les règles Firestore ne sont pas publiées (étape 1.6), ou l'e-mail admin n'est pas confirmé. |
