# CaloTrack PWA — Guide de mise en route (0 €, sans Mac, sans compte développeur)

Contenu du dossier : `index.html` (toute l'app), `manifest.json`, `icon-512.png`,
`icon-192.png`, `apple-touch-icon.png`.

## 1. Mettre en ligne (GitHub Pages, ~10 min, depuis l'iPad)

1. Décompresse le zip dans l'app **Fichiers** de l'iPad (appui sur le zip → il se décompresse).
2. Sur **github.com** : `New repository` → nom `calotrack-app` → **Public** (obligatoire
   pour Pages gratuit ; aucun souci : le code ne contient ni clé ni données, tout reste
   sur les téléphones) → `Create repository`.
3. Sur la page du dépôt : `uploading an existing file` (ou `Add file → Upload files`) →
   sélectionne les **5 fichiers** depuis Fichiers → `Commit changes`.
4. `Settings` → `Pages` → Source : `Deploy from a branch` → Branch `main`, dossier
   `/ (root)` → `Save`.
5. Après 1-2 min, l'URL apparaît en haut de la page Pages :
   `https://TON-PSEUDO.github.io/calotrack-app/`

Mise à jour future de l'app : re-uploader `index.html` (GitHub écrase), puis fermer et
rouvrir CaloTrack sur le téléphone.

## 2. Installer sur les deux iPhones

Sur chaque iPhone : ouvrir l'URL dans **Safari** → bouton **Partager** →
**Sur l'écran d'accueil**. L'icône CaloTrack apparaît ; l'app s'ouvre en plein écran.

Important : l'app installée a son propre stockage, séparé de Safari. Utilisez toujours
l'icône de l'écran d'accueil, pas un onglet Safari, pour que les données restent au même
endroit.

## 3. Premier réglage dans l'app

Onglet **Réglages** :
- **Clé API** : à créer sur console.anthropic.com → API Keys. C'est une facturation à
  l'usage, distincte de l'abonnement claude.ai. Mets un **plafond de dépense** bas
  (Settings → Limits) : un repas analysé coûte une fraction de centime, quelques euros
  par mois couvrent très largement deux utilisateurs. Même clé sur les deux téléphones,
  ou une clé chacun si tu veux suivre la consommation séparément.
- **Modèle** : Sonnet (précis) ou Haiku (éco). Pour du comptage de repas, Haiku suffit
  souvent.
- **Profil** : sexe, âge, taille, poids de secours, objectif kcal/jour. Ta femme remplit
  le sien sur son téléphone — les données ne se mélangent pas.

## 4. Construire le raccourci « Sync Santé » (une fois par iPhone, ~5 min)

Pré-requis côté sources : dans l'app **Whoop** → profil → intégration **Apple Santé**
activée ; dans **Health Mate** (Withings) → partage du poids vers Apple Santé activé.

Dans l'app **Raccourcis** : `+` nouveau raccourci, renomme-le **Sync Santé**, puis
ajoute les actions dans cet ordre (recherche-les par nom) :

1. **Rechercher des échantillons de santé**
   - Type : **Énergie active**
   - Ajouter un filtre : **Date de début** → **est aujourd'hui**
   - Limite : désactivée.
2. **Calculer les statistiques** → opération **Somme** → entrée : les *Échantillons de
   santé* de l'étape 1. (Résultat = tes calories actives du jour.)
3. *(Facultatif — utile pour l'iPhone de ta femme si elle a une Apple Watch)*
   **Rechercher des échantillons de santé** → Type : **Énergie au repos** → même filtre
   « aujourd'hui », puis **Calculer les statistiques** → **Somme**.
4. **Rechercher des échantillons de santé**
   - Type : **Poids**
   - Trier par : **Date de début**, ordre **Le plus récent en premier**, **Limite : 1**.
5. **Texte** — tape exactement ceci, en insérant les variables des étapes précédentes
   via la barre au-dessus du clavier :

   ```
   CTSYNC;date=[Date actuelle];active=[Somme étape 2];basal=[Somme étape 3];poids=[Poids étape 4]
   ```

   - Pour `[Date actuelle]` : insère la variable **Date actuelle**, touche-la, choisis
     **Format : Personnalisé** et saisis `yyyy-MM-dd`.
   - Sans Apple Watch (ton cas), supprime simplement le morceau `basal=[…];`.
   - L'app tolère les virgules décimales et les espaces : pas besoin de formater les
     nombres.
6. **Copier dans le presse-papiers** → entrée : le *Texte* de l'étape 5.
7. *(Confort)* **Afficher une notification** : « Sync copiée — ouvre CaloTrack et touche
   Coller ».

Utilisation quotidienne : lance **Sync Santé** (depuis l'app Raccourcis, un widget, ou
une icône sur l'écran d'accueil : … → Ajouter à l'écran d'accueil), ouvre **CaloTrack**,
touche **« 📥 Coller la synchro Santé »**. iOS demandera une fois l'autorisation de
coller — accepte.

Automatisation possible : Raccourcis → **Automatisation** → **Heure du jour** (ex. 20h30)
→ **Exécuter immédiatement** → raccourci Sync Santé. Le payload attendra dans le
presse-papiers ; il ne restera qu'à ouvrir l'app et coller.

## 5. Usage

- **Dicter un repas** : touche le champ, puis le **micro du clavier** iOS, décris le
  repas, **Analyser les calories**. Vérifie les hypothèses de portion affichées (l'app
  montre aussi le niveau de confiance du modèle), corrige le texte si besoin, puis
  **Enregistrer**.
- **Dépensé** : `(Santé)` = énergie basale mesurée par une Apple Watch + actives ;
  `(mixte)` = métabolisme de base calculé (Mifflin-St Jeor, prorata de la journée) +
  calories actives synchronisées (cas Whoop) ; `(calc.)` = calcul seul, pas de synchro
  aujourd'hui.
- **Semaine** : barres des 7 derniers jours vs objectif (jaune = dépassement), et
  **Plan de rattrapage** généré par Claude à partir de tes chiffres réels.

## 6. Sauvegarde — à lire

Les données vivent dans le stockage web du téléphone. C'est durable, **mais** « Effacer
historique et données de sites » dans Safari, ou la suppression de l'app de l'écran
d'accueil, efface tout. Réflexe : **Réglages → Exporter les données** de temps en temps
(fichier .json, sans la clé API), restaurable via **Restaurer une sauvegarde** — y
compris pour migrer vers la future version native.

## 7. Limites assumées de cette version

- La synchro Santé est semi-manuelle (deux gestes) : c'est le prix du 0 €. La version
  native (dossier `calotrack`, déjà prête) lira HealthKit en direct si tu prends un jour
  le compte Apple Developer.
- Pas de mode hors-ligne : l'app a besoin du réseau (l'analyse des repas aussi, de toute
  façon).
- Les estimations caloriques restent des estimations — l'app affiche les hypothèses pour
  que tu gardes la main.
