# Quai Réception — guide de mise en route (site en ligne)

Un site web hébergé, accessible depuis n'importe quel smartphone (Android **et**
iPhone) par un simple lien — pas de fichier à installer, pas de compte
développeur Apple à payer. On l'ajoute à l'écran d'accueil et il se comporte
comme une vraie application (icône, plein écran, sans barre d'adresse).

Il y a 3 étapes : **Firebase** (données + photos), **EmailJS** (email
automatique), **GitHub Pages** (hébergement du site, gratuit). Comptez 30 à
40 minutes, aucune ligne de code à écrire.

---

## 1. Firebase (base de données + photos) — gratuit

1. Allez sur https://console.firebase.google.com et connectez-vous avec un
   compte Google.
2. **Ajouter un projet** → nommez-le par exemple `quai-reception` → vous
   pouvez désactiver Google Analytics (pas nécessaire) → **Créer le projet**.
3. Menu de gauche : **Build > Firestore Database** → **Créer une base de
   données** → mode **Production** → région `europe-west9 (Paris)` ou
   `europe-west1` → **Activer**.
4. Menu de gauche : **Build > Storage** → **Commencer** → mode **Production**
   → même région → **Terminé**.
5. **Build > Authentication** → **Commencer** → onglet **Sign-in method** →
   **Anonyme** → **Activer** → **Enregistrer**.
6. Roue crantée en haut à gauche → **Paramètres du projet**. Tout en bas de
   l'onglet **Général**, section "Vos applications", cliquez sur l'icône Web
   `</>`. Donnez un nom (ex. `Quai Réception Web`), **Enregistrer
   l'application**. Un objet `firebaseConfig` apparaît — gardez cette page
   ouverte, elle sert à l'étape 3.
7. **Firestore Database > Règles** → remplacez tout le contenu par celui du
   fichier `firestore.rules` fourni → **Publier**.
8. **Storage > Règles** → remplacez tout le contenu par celui du fichier
   `storage.rules` fourni → **Publier**.

## 2. EmailJS (envoi automatique depuis Gmail) — gratuit jusqu'à 200 emails/mois

1. Créez un compte gratuit sur https://www.emailjs.com.
2. **Email Services > Add New Service** → **Gmail** → autorisez l'accès avec
   **lordiffusion@gmail.com**. Notez le **Service ID** (ex. `service_xxxxxxx`).
3. **Email Templates > Create New Template** :
   - **To email** : `routage@lordiffusion.fr`
   - **From name** : `Quai Réception`
   - **Subject** : `Réception marchandise – {{fournisseur}} – {{date}}`
   - **Content** :
     ```
     Nouvelle réception de marchandise.

     Date : {{date}}
     Fournisseur : {{fournisseur}}
     N° commande / BL : {{commande}}
     Référence produit : {{reference}}
     Quantité : {{quantite}}
     Poids : {{poids}}
     Dimensions (L x l x H) : {{dimensions}}
     Conditionnement : {{conditionnement}}
     État : {{etat}}
     Réceptionné par : {{saisi_par}}

     Photo du BL : {{photo_bl_url}}
     Photo du produit : {{photo_produit_url}}
     ```
   - Enregistrez, notez le **Template ID** (ex. `template_xxxxxxx`).
4. **Account > General** → copiez la **Public Key**.

## 3. Compléter la configuration

Ouvrez `index.html` avec un éditeur de texte et remplissez ce bloc, situé
près du début du fichier :

```js
window.FIREBASE_CONFIG = {
  apiKey: "REMPLACER_apiKey",
  authDomain: "REMPLACER_authDomain",
  projectId: "REMPLACER_projectId",
  storageBucket: "REMPLACER_storageBucket",
  messagingSenderId: "REMPLACER_messagingSenderId",
  appId: "REMPLACER_appId"
};

window.EMAILJS_CONFIG = {
  publicKey: "REMPLACER_publicKey",
  serviceId: "REMPLACER_serviceId",
  templateId: "REMPLACER_templateId"
};
```

Remplacez chaque `REMPLACER_...` par les valeurs obtenues en étapes 1 et 2,
puis enregistrez.

## 4. Mettre le site en ligne avec GitHub Pages — gratuit

1. Créez un compte sur https://github.com si besoin.
2. **New repository** → nommez-le `quai-reception` → laissez-le **Public**
   (nécessaire pour que GitHub Pages soit gratuit) → **Create repository**.

   ⚠️ Le site sera accessible par son lien à qui le connaît — il n'apparaît
   dans aucun annuaire ni moteur de recherche, et l'app elle-même exige une
   connexion (authentification anonyme + règles Firebase) pour lire ou
   écrire des données. C'est suffisant pour un usage interne, mais pas un
   vrai coffre-fort si le lien est diffusé largement.

3. Sur la page du dépôt vide, cliquez **uploading an existing file**.
   Glissez-déposez tous les fichiers de ce projet (`index.html`,
   `manifest.json`, `icon-192.png`, `icon-512.png`, `firestore.rules`,
   `storage.rules`, `README.md`) → **Commit changes**.
4. Allez dans **Settings** (onglet en haut du dépôt) → **Pages** (menu de
   gauche). Sous "Build and deployment", choisissez **Deploy from a
   branch**, branche **main**, dossier **/ (root)** → **Save**.
5. Patientez 1 à 2 minutes, puis rechargez cette page **Settings > Pages** :
   un bandeau vert affiche l'adresse de votre site, du type
   `https://votre-nom.github.io/quai-reception/`. C'est le lien à partager
   avec l'équipe.

## 5. Installer l'icône sur les téléphones (Android et iPhone)

1. Ouvrez le lien ci-dessus dans le navigateur du téléphone (Chrome sur
   Android, **Safari** sur iPhone — important, ça ne fonctionne qu'avec
   Safari sur iPhone).
2. **Android (Chrome)** : menu ⋮ → *Ajouter à l'écran d'accueil* / *Installer
   l'application*.
   **iPhone (Safari)** : bouton Partager (carré + flèche) → *Sur l'écran
   d'accueil* → Ajouter.
3. Une icône "Quai Réception" apparaît sur l'écran d'accueil. Elle s'ouvre en
   plein écran, sans barre de navigateur, comme une vraie application.

Répétez l'étape 5 sur chaque téléphone de l'équipe — un seul projet Firebase
suffit pour tout le monde : les données, les photos et l'historique sont
partagés automatiquement entre tous les appareils, Android comme iPhone.

---

## Pour aller plus loin (facultatif)

- **Modifier le site plus tard** : éditez les fichiers directement dans
  GitHub (icône crayon sur chaque fichier) et enregistrez — le site se met à
  jour automatiquement en quelques secondes, sans rien recompiler.
- **Nom de domaine personnalisé** (ex. `reception.lordiffusion.fr`) :
  possible gratuitement depuis **Settings > Pages > Custom domain**, si vous
  avez un nom de domaine.
- **Sécurité** : les téléphones se connectent en mode "anonyme" (sans mot de
  passe individuel), adapté à un usage interne sur des appareils de
  confiance.
- **Limite EmailJS gratuit** : 200 emails/mois.
