# sandbox — Claude Code skill

Un skill Claude Code qui transforme une session en bac à sable pédagogique.
Tu donnes un sujet (`/sandbox DDD`, `/sandbox useEffect`, etc.) et Claude génère un mini-projet incomplet à compléter, un plan d'apprentissage progressif, puis adopte un mode professeur socratique pour toute la session.

## Installation

1. Clone ce repo dans le dossier `plugins/` de ton projet (ou dans `~/.claude/plugins/` pour un usage global) :

```bash
# Dans ton projet
git clone https://github.com/hazemgherissi/claude-sandbox-skill plugins/sandbox

# Ou globalement
git clone https://github.com/hazemgherissi/claude-sandbox-skill ~/.claude/plugins/sandbox
```

2. Dans ton `settings.json` (ou `settings.local.json`), référence le plugin :

```json
{
  "plugins": ["plugins/sandbox"]
}
```

3. Redémarre Claude Code — le skill `/sandbox` est maintenant disponible.

## Utilisation

```
/sandbox DDD
/sandbox tests unitaires
/sandbox React useEffect
/sandbox injection de dépendances
```

Claude génère un dossier `sandbox-[sujet]/` dans le répertoire courant avec :
- Des fichiers de code partiellement écrits (`// TODO:` là où tu travailles)
- Un `README-sandbox.md` avec le plan d'apprentissage et les exercices
- Un `progression.md` pour suivre ta progression entre les sessions

## Ce qui est hardcodé — et pourquoi ça compte

### Le profil apprenant

Le skill est actuellement calibré pour ce profil dans `skills/sandbox/SKILL.md` :

```
- Débutant (moins d'un an d'expérience)
- Stack : React (Frontend) + ASP.NET Core (Backend)
```

Si ton profil est différent (intermédiaire, autre stack), modifie cette section directement dans le fichier.

### Les templates de structure de dossiers

Le skill contient deux templates précis pour générer le squelette de projet :

| Template | Ce qu'il génère |
|---|---|
| **ASP.NET Core** | Solution `.NET` buildable avec `dotnet CLI`, structure `src/` + `tests/`, packages `Moq` + `FluentAssertions` |
| **React** | Dossiers `components/`, `hooks/`, `services/` |

Pour tout autre sujet, Claude improvise la structure sans template — ça marche, mais c'est moins fiable et moins cohérent.

## Ajouter un template pour une autre techno

Si tu travailles avec Python, Node.js, Vue, etc., tu peux ajouter un template dans `skills/sandbox/SKILL.md` dans la section **Étape 2 — Génération du bac à sable**.

Exemple pour Node.js / Express :

````markdown
### Structure pour un sujet Node.js / Express :

```bash
mkdir sandbox-[sujet] && cd sandbox-[sujet]
npm init -y
npm install express
npm install --save-dev jest supertest
```

```
sandbox-[sujet]/
├── src/
│   ├── routes/
│   │   └── [ressource].routes.js    <- routes fournies
│   ├── services/
│   │   └── [ressource].service.js   <- à compléter
│   └── app.js                       <- setup Express fourni
├── tests/
│   └── [ressource].test.js          <- tests à écrire
└── README-sandbox.md
```
````

Il faut aussi mettre à jour la question de l'Étape 1 pour que Claude sache détecter cette stack :

```markdown
1. **Identifie la stack concernée** : React, ASP.NET Core, Node.js, ou autre ?
```

Et dans la section **Format des réponses** du mode professeur, ajouter la précision de couche correspondante :

```markdown
- Préciser la couche en Node.js (route / service / middleware)
```

## Structure du plugin

```
sandbox/
├── .claude-plugin/
│   └── plugin.json          <- métadonnées (nom, auteur)
├── skills/
│   └── sandbox/
│       └── SKILL.md         <- le skill complet
└── README.md
```
