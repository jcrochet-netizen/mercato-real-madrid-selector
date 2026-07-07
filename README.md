# Mercato Real Madrid 2026 — Outil drag & drop « Je garde / Je vends »

Outil interactif (1 seul fichier `index.html`, sans dépendance) qui permet aux supporters
de choisir, par glisser-déposer, quels joueurs de l'effectif **Real Madrid 2025/2026**
ils gardent ou vendent pour la saison 2026/2027, puis de **partager** leur sélection.

- Effectif récupéré via l'**API Sportmonks** (photos, noms, postes), figé dans le fichier.
- Valeurs marchandes et salaires **estimés / provisoires** (à ajuster dans le code).
- Drag & drop **souris + tactile** (mobile), boutons rapides « Garder / Vendre ».
- Écran de résultat avec bilan (Salaires économisés, Ventes, Options d'achat, Solde net).
- Partage **X, Facebook, WhatsApp, Web Share**, copier le texte, et **téléchargement d'une image PNG**.
- Détection automatique de l'URL de la page hôte pour les partages (postMessage + referrer).

Jumeau OM : https://github.com/jcrochet-netizen/mercato-om-selector

## Héberger / intégrer

Hébergé sur **GitHub Pages** : `https://jcrochet-netizen.github.io/mercato-real-madrid-selector/`

Embed WordPress (bloc HTML personnalisé) :

```html
<iframe
  id="mercato-rm"
  src="https://jcrochet-netizen.github.io/mercato-real-madrid-selector/"
  scrolling="no"
  style="width:100%;border:0;display:block;min-height:1600px;overflow:hidden;"
  loading="lazy"
  title="Mercato Real Madrid 2026 — Je garde ou je vends ?"></iframe>

<script>
(function () {
  var WIDGET_ORIGIN = 'https://jcrochet-netizen.github.io';
  var f = document.getElementById('mercato-rm');
  function sendUrl() {
    try { f.contentWindow.postMessage({ type: 'mercato-rm-parent-url', url: location.href }, WIDGET_ORIGIN); } catch (e) {}
  }
  f.addEventListener('load', sendUrl);
  if (f.contentWindow) { sendUrl(); }
  window.addEventListener('message', function (e) {
    if (e.origin !== WIDGET_ORIGIN) return;
    if (!e.data) return;
    if (e.data.type === 'mercato-rm-ready') { sendUrl(); }
    if (e.data.type === 'mercato-rm-height') { f.style.height = e.data.height + 'px'; }
  });
})();
</script>
```

## Personnaliser (en haut du `<script>` dans `index.html`)

- **`PLAYERS`** : effectif. Champ **`val`** = valeur marchande estimée en M€ (provisoire).
- **`SALARIES`** : salaire annuel brut estimé (M€/an) par id de joueur (provisoire).
- **`SHORT`** : libellé court d'affichage par id (ex. Vinicius, distinguer les deux García).
- **`LOAN` / `EXERCISED` / `EXPIRING` / `RETURNING`** : statuts spéciaux (prêté, option obligatoire
  levée, fin de contrat, retour de prêt). Vides par défaut pour le Real (tous joueurs de l'effectif).
- **`CONFIG.votesUrl`** : endpoint Google Apps Script pour les votes « Avis de la communauté ».
  Vide = fonctionnalité désactivée. À brancher sur un Sheet **dédié au Real** (distinct de l'OM).
- **`CONFIG.shareUrl`** : fallback si l'URL de la page hôte n'est pas détectée.

## Mettre à jour l'effectif (optionnel)

```
GET https://api.sportmonks.com/v3/football/teams/3468?api_token=TON_TOKEN&include=players.player.position;players.player.detailedPosition
```

(`3468` = ID du Real Madrid.)

---
*Données : Sportmonks. Valeurs marchandes et salaires estimés (indicatifs). Salaires : capology.com.*
