# Le Ticket — guide de mise en ligne

Cette version utilise **Firebase Authentication** (vrai email/mot de passe) et
**Firestore** (base de données) au lieu du stockage propre à Claude. Il faut
donc créer votre propre projet Firebase avant de publier.

## 1. Créer le projet Firebase

1. Allez sur https://console.firebase.google.com et créez un projet (gratuit).
2. Dans **Authentication → Sign-in method**, activez le fournisseur **E-mail/Mot de passe**.
3. Dans **Firestore Database**, cliquez sur **Créer une base de données** (mode production).
4. Dans **Paramètres du projet → Vos applications**, ajoutez une application **Web**
   (icône `</>`), donnez-lui un nom, puis copiez l'objet `firebaseConfig` affiché.

## 2. Coller votre configuration

Ouvrez `public/index.html`, cherchez ce bloc vers le milieu du fichier, et
remplacez les valeurs par celles copiées à l'étape 1 :

```js
const firebaseConfig = {
  apiKey: "VOTRE_API_KEY",
  authDomain: "VOTRE_PROJET.firebaseapp.com",
  projectId: "VOTRE_PROJET",
  storageBucket: "VOTRE_PROJET.appspot.com",
  messagingSenderId: "VOTRE_SENDER_ID",
  appId: "VOTRE_APP_ID"
};
```

Remplacez aussi `VOTRE_PROJET` dans `.firebaserc` par l'ID exact de votre projet
(visible dans Paramètres du projet).

## 3. Mettre le code sur GitHub

Depuis le dossier `deploy/`, dans un terminal :

```bash
git init
git add .
git commit -m "Premier envoi : Le Ticket"
git branch -M main
git remote add origin https://github.com/VOTRE_UTILISATEUR/le-ticket.git
git push -u origin main
```

(Créez d'abord un dépôt vide sur https://github.com/new — sans README ni
.gitignore pour éviter les conflits.)

## 4. Déployer sur Firebase Hosting

Toujours dans le dossier `deploy/` :

```bash
npm install -g firebase-tools
firebase login
firebase deploy
```

`firebase login` ouvre votre navigateur pour vous authentifier avec le compte
Google propriétaire du projet Firebase. À la fin de `firebase deploy`, l'outil
affiche l'URL publique (du type `https://votre-projet.web.app`).

## 5. Publier les mises à jour

À chaque modification :

```bash
git add .
git commit -m "Description du changement"
git push
firebase deploy
```

## Notes

- Les règles Firestore (`firestore.rules`) garantissent que chaque personne ne
  voit que sa propre liste — personne ne peut lire celle d'un autre compte.
- Si vous préférez héberger uniquement sur **GitHub Pages** plutôt que Firebase
  Hosting, c'est possible aussi (Settings → Pages sur votre dépôt GitHub) : le
  fichier `public/index.html` fonctionne tel quel, seul Firebase Hosting
  devient alors inutile — gardez simplement Auth + Firestore actifs côté Firebase.
