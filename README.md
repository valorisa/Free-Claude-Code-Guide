<!-- markdownlint-disable MD013 -->
# Free Claude Code - Guide Complet d'Installation et d'Utilisation

[![CI](https://github.com/valorisa/Free-Claude-Code-Guide/actions/workflows/ci.yml/badge.svg)](https://github.com/valorisa/Free-Claude-Code-Guide/actions/workflows/ci.yml)
[![Lint](https://github.com/valorisa/Free-Claude-Code-Guide/actions/workflows/lint.yml/badge.svg)](https://github.com/valorisa/Free-Claude-Code-Guide/actions/workflows/lint.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yelow.svg)](https://opensource.org/licenses/MIT)
[![GitHub stars](https://img.shields.io/github/stars/valorisa/Free-Claude-Code-Guide?style=social)](https://github.com/valorisa/Free-Claude-Code-Guide)
[![GitHub forks](https://img.shields.io/github/forks/valorisa/Free-Claude-Code-Guide?style=social)](https://github.com/valorisa/Free-Claude-Code-Guide/network/members)
[![GitHub watchers](https://img.shields.io/github/watchers/valorisa/Free-Claude-Code-Guide?style=social)](https://github.com/valorisa/Free-Claude-Code-Guide/watchers)
[![Dependabot Alerts](https://img.shields.io/github/dependabot/valorisa/Free-Claude-Code-Guide)](https://github.com/valorisa/Free-Claude-Code-Guide/security/dependabot)

> **IMPORTANT** : Ce guide est un "pense-bête" exhaustif créé après une session d'installation complète sur macOS Sequoia avec VSCode.
> Il contient toutes les subtilités, nuances et décisions techniques prises lors de l'installation.
> Il sert de référence complète pour l'utilisation de Free Claude Code.
> C'est un guide de référence technique. Il est très détaillé. Il est très complet.
> Il est très exhaustif.

## Table des matières

1. [Instructions par Système d'Exploitation](#instructions-par-système-dexploitation)
2. [Qu'est-ce que Free Claude Code ?](#quest-ce-que-free-claude-code-)
3. [Architecture et Fonctionnement](#architecture-et-fonctionnement)
4. [Emplacement du Projet](#emplacement-du-projet)
5. [Pré-requis et Installation](#pré-requis-et-installation)
6. [Configuration Détaillée](#configuration-détaillée)
7. [Comprendre le Routage des Modèles](#comprendre-le-routage-des-modèles)
8. [Intégration VSCode](#intégration-vscode)
9. [Utilisation Quotidienne](#utilisation-quotidienne)
10. [Dépannage et Subtilités](#dépannage-et-subtilités)
11. [Résumé des Commandes Essentielles](#résumé-des-commandes-essentielles)
12. [Ressources et Liens](#ressources-et-liens)
13. [Avertissements et Bonnes Pratiques](#avertissements-et-bonnes-pratiques)

---

## Instructions par Système d'Exploitation

### Windows 10/11 Enterprise

**Particularités** : Les environnements d'entreprise imposent souvent des restrictions.
Cela inclut le proxy, l'antivirus, et les politiques de groupe.
Une configuration spécifique est nécessaire.

**Installation de uv** (PowerShell en tant qu'administrateur) :

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
uv self update
uv python install 3.14
```

**Configuration du proxy d'entreprise** :
Si votre entreprise impose un proxy sortant, éditez le fichier `.env` :

```dotenv
NVIDIA_NIM_PROXY="http://utilisateur:motdepasse@proxy-entreprise:8080"
```

**Cloner le dépôt** (PowerShell) :

```powershell
cd $env:USERPROFILE\Projets
git clone https://github.com/Alishahryar1/free-claude-code.git
cd free-claude-code
Copy-Item .env.example .env
```

**Lancer le proxy et Claude Code** (deux terminaux PowerShell) :

```powershell
# Terminal1
cd $env:USERPROFILE\Projets\free-claude-code
uv run uvicorn server:app --host 0.0.0.0 --port 8082

# Terminal2
$env:ANTHROPIC_AUTH_TOKEN="freecc"
$env:ANTHROPIC_BASE_URL="http://localhost:8082"
claude

# En une seule commande (PowerShell)
$env:ANTHROPIC_AUTH_TOKEN="freecc"; $env:ANTHROPIC_BASE_URL="http://localhost:8082"; claude
```

**⚠️ Notes spécifiques Windows Enterprise** :

- Ajoutez `uv` au PATH manuellement si nécessaire : `%USERPROFILE%\.cargo\bin` ou `%USERPROFILE%\.local\bin`
- Désactivez temporairement l'antivirus si le téléchargement de Python 3.14 échoue
- Les dossiers avec espaces peuvent poser problème : préférez `$env:USERPROFILE\Projets` à `$env:USERPROFILE\Mon Nom\Projets`

---

### macOS (Sequoia et versions antérieures)

**Particularités** : macOS est le système le plus simple pour ce projet. Homebrew est recommandé pour les dépendances.

**Installation de uv** (Terminal zsh/bash) :

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
uv self update
uv python install 3.14
```

**Cloner le dépôt** :

```bash
cd $HOME/Projets
git clone https://github.com/Alishahryar1/free-claude-code.git
cd free-claude-code
cp .env.example .env
```

**Lancer le proxy et Claude Code** :

```bash
# Terminal1
cd $HOME/Projets/free-claude-code
uv run uvicorn server:app --host 0.0.0.0 --port 8082

# Terminal2
ANTHROPIC_AUTH_TOKEN="freecc" ANTHROPIC_BASE_URL="http://localhost:8082" claude

# En une seule commande
ANTHROPIC_AUTH_TOKEN="freecc" ANTHROPIC_BASE_URL="http://localhost:8082" claude
```

**⚠️ Notes spécifiques macOS** :

- Si vous utilisez un Mac avec puce Apple Silicon (M1/M2/M3/M4), Python 3.14 s'installera en version `aarch64` (natif)
- Les permissions Gatekeeper peuvent bloquer l'exécution : allez dans Préférences > Sécurité pour autoriser
- Pour VSCode, l'extension utilise les variables d'environnement configurées dans `settings.json`

---

### Linux (Ubuntu/Debian/Fedora/Arch)

**Particularités** : Linux offre la meilleure compatibilité. Aucune restriction système majeure.

**Installation de uv** (Terminal bash/zsh) :

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
uv self update
uv python install 3.14
```

**Cloner le dépôt** :

```bash
cd ~/Projets
git clone https://github.com/Alishahryar1/free-claude-code.git
cd free-claude-code
cp .env.example .env
```

**Lancer le proxy et Claude Code** :

```bash
# Terminal1
cd ~/Projets/free-claude-code
uv run uvicorn server:app --host 0.0.0.0 --port 8082

# Terminal2
ANTHROPIC_AUTH_TOKEN="freecc" ANTHROPIC_BASE_URL="http://localhost:8082" claude

# En une seule commande
ANTHROPIC_AUTH_TOKEN="freecc" ANTHROPIC_BASE_URL="http://localhost:8082" claude
```

**⚠️ Notes spécifiques Linux** :

- Pour les serveurs sans GUI, utilisez `tmux` ou `screen` pour laisser le proxy tourner en arrière-plan :

  ```bash
  tmux new -s proxy
  uv run uvicorn server:app --host 0.0.0.0 --port 8082
  # Ctrl+B puis D pour détacher
  # tmux attach -t proxy pour revenir
  ```

- Si vous utilisez `firewalld` (Fedora/RHEL) ou `ufw` (Ubuntu), ouvrez le port 8082 :

  ```bash
  sudo ufw allow 8082/tcp
  # ou
  sudo firewall-cmd --permanent --add-port=8082/tcp && sudo firewall-cmd --reload
  ```

- L'intégration VSCode fonctionne de la même manière que sur macOS

> **💡 Note importante pour le test** : Vous n'avez **pas besoin** d'être dans un dossier spécifique comme `$HOME/Projets/free-claude-code-guide` ou `$HOME/Projets/free-claude-code`. Vous pouvez lancer Claude Code depuis **n'importe quel dossier**.
>
> **Ce qu'il faut vérifier** :
>
> 1. **Vérifier que le proxy tourne** (port 8082) :
>
>    ```bash
>    lsof -i :8082
>    ```
>
>    S'il ne tourne plus, relancez-le :
>
>    ```bash
>    cd $HOME/Projets/free-claude-code && source .venv/bin/activate && python server.py --port 8082 &
>    ```
>
> 2. **Lancer Claude Code avec les variables** (depuis n'importe où) :
>
>    ```bash
>    export ANTHROPIC_BASE_URL="http://localhost:8082"
>    export ANTHROPIC_AUTH_TOKEN="freecc"
>    claude
>    ```
>
>    Ou en une seule commande :
>
>    ```bash
>    ANTHROPIC_BASE_URL="http://localhost:8082" ANTHROPIC_AUTH_TOKEN="freecc" claude
>    ```

---

## Qu'est-ce que Free Claude Code ?

**Free Claude Code** (dépôt : `Alishahryar1/free-claude-code`) est un **proxy**. Il redirige les appels API d'Anthropic vers des fournisseurs gratuits ou personnels, notamment **NVIDIA NIM**.
C'est un projet open-source. Il est très utilisé. Il est performant.
C'est un projet open-source.

### Ce que ça fait :

- Intercepte les requêtes que Claude Code envoie normalement aux serveurs d'Anthropic
- Les redirige vers NVIDIA NIM (ou OpenRouter, DeepSeek, LM Studio, etc.)
- Permet d'utiliser Claude Code CLI avec des modèles **gratuits** ou **personnels**

### Ce que ça N'EST PAS :

- Ce n'est **pas** une version crackée de Claude Code
- Ce n'est **pas** un remplacement des modèles Claude d'Anthropic
- Les "tiers" Opus/Sonnet/Haiku de Claude Code sont **routés** vers d'autres modèles (GLM-4.7, Kimi K2, etc.)

---

## Architecture et Fonctionnement

```text
┌─────────────────┐         ┌──────────────────────┐         ┌─────────────────┐
│  Claude Code    │         │  Free Claude Code    │         │ Provider        │
│  CLI ou VSCode  │────────>│  Proxy (port 8082)   │────────>│ (OpenRouter,    │
│                 │         │                      │         │  NVIDIA NIM,    │
└─────────────────┘         └──────────────────────┘         │  Local, etc.)   │
                                                              └─────────────────┘
```

### Flux de données :

1. **Claude Code** envoie une requête API Anthropic (format Messages API)
2. **Free Claude Code** (proxy local) intercepte la requête sur `http://localhost:8082`
3. Le proxy **traduit** la requête au format attendu par le fournisseur
4. Le **provider** (OpenRouter, NVIDIA NIM, etc.) traite la requête et renvoie la réponse
5. Le proxy **reformate** la réponse au format Anthropic

### Points subtils à comprendre :

#### 1. Le concept de "Proxy Sortant" (NVIDIA_NIM_PROXY)

```dotenv
NVIDIA_NIM_PROXY=""
```

- Cette option sert à ajouter un **proxy intermédiaire** entre Free Claude Code et NVIDIA NIM
- Exemple : Si vous êtes en entreprise avec un proxy sortant `http://proxy:8080`
- **Usage personnel à la maison** : Laissez vide
- **Ce n'est PAS un double proxy** entre Claude Code et Free Claude Code (ils sont sur la même machine)

#### 2. Le routage Opus/Sonnet/Haiku

Claude Code demande des modèles par "tiers" (Opus = complexe, Sonnet = équilibré, Haiku = rapide). Ces tiers sont **virtuels** dans ce contexte. Ils sont routés vers des vrais modèles :
C'est une particularité du projet. C'est unique.

- `MODEL_OPUS` → Normalement pour tâches complexes (ex: Kimi K2)
- `MODEL_SONNET` → Équilibré (ex: GLM-4.7)
- `MODEL_HAIKU` → Rapide (ex: MiniMax M1)
- `MODEL` → Fallback si les autres sont vides

---

## Emplacement du Projet

**Dossier du projet Free Claude Code** :

```text
$HOME/Projets/free-claude-code
```

**Dossier de ce guide** :

```text
$HOME/Projets/free-claude-code-guide
```

---

## Pré-requis et Installation

### Étape 1 : Installer uv (gestionnaire de paquets Python moderne)

`uv` est un outil équivalent à `pip` mais beaucoup plus rapide. Il gère aussi les versions de Python. Il est recommandé de l'utiliser pour ce projet. C'est un outil moderne. Il est performant. Il est très rapide.
Il est très performant.
Il est très performant.

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Vérifier l'installation :

```bash
which uv
# Devrait afficher : $HOME/.local/bin/uv
```

### Étape 2 : Installer Python 3.14

Free Claude Code nécessite Python 3.14 (version spécifiée dans le projet).

```bash
uv python install 3.14
```

### Étape 3 : Cloner le dépôt

```bash
cd $HOME/Projets
git clone https://github.com/Alishahryar1/free-claude-code.git
```

### Étape 4 : Installer les dépendances

```bash
cd $HOME/Projets/free-claude-code
uv sync
```

**Ce que cette commande fait** :

- Crée un environnement virtuel Python dans `.venv`
- Installe toutes les dépendances listées dans `pyproject.toml`
- Les dépendances incluent : FastAPI, httpx, OpenAI, etc.

---

## Configuration Détaillée

### Le fichier `.env`

Le fichier `.env` contient toute la configuration. Il est situé à :

```text
$HOME/Projets/free-claude-code/.env
```

#### 1. Choix du Provider (OBLIGATOIRE)

Free Claude Code supporte 6 providers. **OpenRouter** est recommandé pour commencer (modèles gratuits sans carte bancaire).

**Option A : OpenRouter (Recommandé - Gratuit)**

Obtenir une clé sur : <https://openrouter.ai/keys>

```dotenv
OPENROUTER_API_KEY="sk-or-votre-cle-ici"
MODEL_OPUS="open_router/google/gemma-4-26b-a4b-it:free"
MODEL_SONNET="open_router/google/gemma-4-26b-a4b-it:free"
MODEL_HAIKU="open_router/google/gemma-4-26b-a4b-it:free"
MODEL="open_router/google/gemma-4-26b-a4b-it:free"
```

**Option B : NVIDIA NIM (Alternative)**

Obtenir une clé sur : <https://build.nvidia.com/settings/api-keys>

```dotenv
NVIDIA_NIM_API_KEY="nvapi-votre-clé-ici"
MODEL_OPUS="nvidia_nim/meta/llama-3.1-70b-instruct"
MODEL_SONNET="nvidia_nim/z-ai/glm4.7"
MODEL_HAIKU="nvidia_nim/meta/llama-3.1-8b-instruct"
MODEL="nvidia_nim/z-ai/glm4.7"
```

**⚠️ Sécurité** : Ne jamais partager votre fichier `.env` avec vos clés. Ajoutez `.env` à votre `.gitignore`.

#### 2. Format des Modèles

**Format** : `provider_type/model/name`

- `open_router` ou `nvidia_nim` = fournisseur
- `google` ou `z-ai` = organisation
- `gemma-4-26b-a4b-it:free` ou `glm4.7` = nom du modèle

#### 3. Token d'authentification

```dotenv
ANTHROPIC_AUTH_TOKEN="freecc"
```

Ce token est **local** et **fictif**. Claude Code l'envoie au proxy, et le proxy le renvoie tel quel. Vous pouvez mettre n'importe quelle valeur (ex: "toto", "1234"). "freecc" est la convention du projet. C'est un token de test.
C'est un token de test. Il n'est pas sensible.

#### 4. Options de "Thinking" (Raisonnement)

```dotenv
ENABLE_MODEL_THINKING=true
```

Cela active les blocs de réflexion. Les options par modèle héritent de cette valeur.

#### 5. Configuration du serveur

```dotenv
PROVIDER_RATE_LIMIT=1
PROVIDER_RATE_WINDOW=3
PROVIDER_MAX_CONCURRENCY=5
```

- `PROVIDER_RATE_LIMIT` : Nombre de requêtes
- `PROVIDER_RATE_WINDOW` : Sur combien de secondes
- `PROVIDER_MAX_CONCURRENCY` : Requêtes simultanées max

---

## Comprendre le Routage des Modèles

### La confusion "Opus 4.7"

**⚠️ Point important** : Il n'existe pas de modèle "Claude Opus 4.7".

- **GLM-4.7** est un modèle de **Zhipu AI** (entreprise chinoise)
- **Claude Opus** est un modèle d'**Anthropic** (entreprise américaine)
- Ils n'ont **rien à voir** l'un avec l'autre

Dans Free Claude Code :

- Quand Claude Code "demande" Opus → Le proxy envoie la requête vers le modèle défini dans `MODEL_OPUS`
- Si vous mettez `MODEL_OPUS="nvidia_nim/z-ai/glm4.7"`, alors "Opus" = GLM-4.7 (c'est juste un alias)

### Comparaison des modèles par Provider

**Provider : NVIDIA NIM**

| Modèle | Identifiant complet | Points forts | Points faibles |
| ------- | ------------------- | ------------ | -------------- |
| **GLM-4.7** | `nvidia_nim/z-ai/glm4.7` | Équilibré, fiable | - |
| **Kimi K2** | `nvidia_nim/moonshotai/kimi-k2` | Très capable | Moins testé |
| **MiniMax M1** | `nvidia_nim/minimax/minimax-m1` | Rapide | Moins puissant |

**Provider : OpenRouter (Modèles gratuits `:free`)**

| Modèle | Identifiant complet | Points forts | Points faibles |
| ------- | ------------------- | ------------ | -------------- |
| **Gemma-4** | `open_router/google/gemma-4-26b-a4b-it:free` | Léger, gratuit | Limites de taux |
| **Llama 3.1** | `open_router/meta-llama/llama-3.1-8b-instruct:free` | Standard, stable | - |

**Recommandations** :

- **OpenRouter (gratuit)** : Idéal pour débuter sans carte bancaire (`google/gemma-4-26b-a4b-it:free`)
- **NVIDIA NIM** : Pour une utilisation plus poussée (nécessite une clé API NVIDIA)

---

## Intégration VSCode

### Extension Claude Code

Si vous utilisez l'extension Claude Code dans VSCode, elle doit pointer vers le proxy local.

**Fichier** : `$HOME/Library/Application Support/Code/User/settings.json`

**Config ajoutée** :

```json
"claudeCode.environmentVariables": [
    { "name": "ANTHROPIC_BASE_URL", "value": "http://localhost:8082" },
    { "name": "ANTHROPIC_AUTH_TOKEN", "value": "freecc" }
]
```

**⚠️ Points subtils** :

1. **Pas de `/v1`** à la fin de l'URL
2. **Recharger l'extension** : Après modification de `settings.json`
3. **Le proxy doit tourner** : L'extension ne fonctionnera que si le proxy est actif

### Si l'extension affiche un écran de connexion

- Vérifiez que les variables d'environnement sont bien dans `settings.json`
- Rechargez l'extension ou redémarrez VSCode
- L'écran de connexion peut apparaître une fois, mais le proxy local sera utilisé après

---

## Utilisation Quotidienne

### Méthode recommandée : Deux terminaux

C'est la méthode la plus stable et la plus facile à comprendre.

#### Terminal 1 : Lancer le proxy (serveur)

```bash
cd $HOME/Projets/free-claude-code
uv run uvicorn server:app --host 0.0.0.0 --port 8082
```

**Ce que cette commande fait** :

- `uv run` : Exécute la commande dans l'environnement virtuel du projet
- `uvicorn` : Serveur web ASGI (comme gunicorn mais pour Python asynchrone)
- `server:app` : Le fichier `server.py` et la variable `app` (l'application FastAPI)
- `--host 0.0.0.0` : Écoute sur toutes les interfaces (pas seulement localhost)
- `--port 8082` : Port d'écoute

**⚠️ Laissez ce terminal ouvert** ! Le proxy doit rester actif.

#### Terminal 2 : Lancer Claude Code (client)

```bash
ANTHROPIC_AUTH_TOKEN="freecc" ANTHROPIC_BASE_URL="http://localhost:8082" claude
```

**Ce que cette commande fait** :

- Définit `ANTHROPIC_AUTH_TOKEN` pour cette session uniquement
- Définit `ANTHROPIC_BASE_URL` pour rediriger vers le proxy local
- Lance `claude` (CLI de Claude Code)

**Note** : Claude Code doit être installé séparément. Si ce n'est pas le cas :

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

### Méthode alternative : Un seul terminal

Si vous préférez n'utiliser qu'un seul terminal :

```bash
cd $HOME/Projets/free-claude-code
uv run uvicorn server:app --host 0.0.0.0 --port 8082 &
ANTHROPIC_AUTH_TOKEN="freecc" ANTHROPIC_BASE_URL="http://localhost:8082" claude
```

**Explication du `&`** :

- Le `&` à la fin de la première commande la met en **arrière-plan**
- Le terminal peut alors exécuter la deuxième commande
- Le proxy tourne en tâche de fond

**Pour arrêter le proxy plus tard** :

```bash
kill %1
# ou
pkill -f uvicorn
```

---

## Dépannage et Subtilités

### Le proxy ne démarre pas

**Vérifier que le fichier `.env` est correct** :

```bash
cat $HOME/Projets/free-claude-code/.env | grep NVIDIA_NIM_API_KEY
```

La clé ne doit pas être vide.

**Vérifier que le port 8082 n'est pas utilisé** :

```bash
lsof -i :8082
```

Si quelque chose utilise le port, changez le port dans la commande (`--port 8083` par exemple).

### Claude Code ne se connecte pas

**Symptôme** : Claude Code affiche une erreur de connexion.

**Vérifications** :

1. Le proxy tourne (Terminal 1 actif)
2. L'URL est correcte : `http://localhost:8082` (pas de `/v1`)
3. Le token est défini : `ANTHROPIC_AUTH_TOKEN="freecc"`

### Les modèles ne répondent pas comme attendu

**Vérifier les logs du proxy** (Terminal 1) :

- Les erreurs de l'API NVIDIA NIM s'affichent ici
- Vérifiez que votre clé API est valide
- Vérifiez que le modèle demandé existe sur NVIDIA NIM

### Réinitialiser complètement

```bash
cd $HOME/Projets/free-claude-code
rm .env
cp .env.example .env
# Puis ré-éditer .env avec votre clé
```

### Question fréquente : "Puis-je utiliser de vrais modèles Claude ?"

**Oui, mais** :

- Il faut utiliser le provider `open_router` avec une clé API
- Ce n'est **pas gratuit** (OpenRouter facture à l'usage)
- Dans ce cas, configurez :

  ```dotenv
  OPENROUTER_API_KEY="sk-or-votre-clé"
  MODEL_OPUS="open_router/anthropic/claude-3-opus"
  ```

---

## Résumé des Commandes Essentielles

### Installation (à faire une seule fois)

```bash
# Installer uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# Installer Python 3.14
uv python install 3.14

# Cloner et installer le projet
cd $HOME/Projets
git clone https://github.com/Alishahryar1/free-claude-code.git
cd free-claude-code
uv sync
```

### Configuration (à faire une seule fois)

```bash
# Éditer le fichier .env
code $HOME/Projets/free-claude-code/.env
# Ajouter : NVIDIA_NIM_API_KEY="nvapi-votre-clé"
```

### Utilisation quotidienne

```bash
# Terminal 1 : Démarrer le proxy
cd $HOME/Projets/free-claude-code
uv run uvicorn server:app --host 0.0.0.0 --port 8082

# Terminal 2 : Démarrer Claude Code
ANTHROPIC_AUTH_TOKEN="freecc" ANTHROPIC_BASE_URL="http://localhost:8082" claude
```

### Vérifications

```bash
# Voir les processus uvicorn
ps aux | grep uvicorn

# Voir ce qui écoute sur le port 8082
lsof -i :8082

# Tester le proxy (curl)
curl http://localhost:8082/v1/models
```

---

## Ressources et Liens

- **Dépôt Free Claude Code** : [GitHub](https://github.com/Alishahryar1/free-claude-code)
- **Obtenir une clé NVIDIA NIM** : [NVIDIA](https://build.nvidia.com/settings/api-keys)
- **Documentation uv** : [Docs](https://docs.astral.sh/uv/)
- **Claude Code officiel** : [Site](https://claude.ai/) (pour installer le CLI)

---

## Avertissements et Bonnes Pratiques

1. **Sécurité** : Ne commitez jamais votre fichier `.env` avec la clé API
2. **Mises à jour** : Free Claude Code est un projet actif, faites `git pull` régulièrement
3. **Limites de NVIDIA NIM** : Le tier gratuit a une limite de 40 req/min
4. **Stabilité** : GLM-4.7 est le modèle par défaut car le plus testé avec Claude Code CLI
5. **VSCode** : L'extension nécessite que le proxy tourne en arrière-plan

---

*Dernière mise à jour : Mai 2026*
*Guide créé par valorisa après session d'installation complète sur macOS Sequoia*
 
