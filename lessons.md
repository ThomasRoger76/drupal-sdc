# Leçons — drupal-sdc

Erreurs SDC découvertes en projet Drupal 10.3+/D11.

---

### 2026-05-16 — Composant non découvert — nom de fichier incorrect

- **Symptôme :** `Component 'mon_theme:card' not found` dans Twig
- **Cause :** Le fichier `.twig` ne porte pas le même nom que le répertoire (`card/mon-template.html.twig` au lieu de `card/card.twig`)
- **Correct :** Le fichier Twig DOIT avoir le même nom que le répertoire : `card/card.twig`
- **Prévention :** Règle : `components/NOM/NOM.twig` — exactement le même nom partout

### 2026-05-16 — Props non typées — erreur silencieuse en dev

- **Symptôme :** Une prop `url` reçoit un objet `Url` Drupal au lieu d'une string — Twig rend un objet vide
- **Cause :** Props non typées dans `.component.yml` — aucune validation ni conversion
- **Correct :** Déclarer `url: { type: string, format: uri }` dans les props + convertir l'objet avant de passer la prop
- **Prévention :** Activer `sdc.debug: true` dans services.yml en développement — la validation stricte prévient ces erreurs

### 2026-05-16 — JS double-initialisation — Drupal.behaviors non utilisé

- **Symptôme :** Le comportement JavaScript du composant s'exécute plusieurs fois
- **Cause :** Code JS dans un `document.ready()` ou `window.onload` au lieu de `Drupal.behaviors`
- **Correct :** Pattern obligatoire : `Drupal.behaviors.monComposant = { attach: function(context) { ... } }`
- **Prévention :** Toujours utiliser `Drupal.behaviors` dans le JS SDC

### 2026-05-16 — `cache:rebuild` requis après création d'un composant

- **Symptôme :** Le nouveau composant SDC retourne une erreur "Component not found" même si les fichiers existent
- **Cause :** Le registre SDC est mis en cache — Drupal ne découvre pas les nouveaux composants automatiquement
- **Correct :** `drush cr` après chaque création ou renommage de composant SDC
- **Prévention :** En développement, ajouter `auto_reload: true` dans `services.yml` pour Twig + `drush cr` après création
