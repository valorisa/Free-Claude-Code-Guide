# Free Claude Code - Guide d'installation et d'utilisation

> Guide mémo pour utiliser Free-Claude-Code avec NVIDIA NIM sur macOS Sequoia (VSCode)

## 📁 Emplacement du projet
```
/Users/valorisa/Projets/free-claude-code
```

## ✅ Installation (déjà faite)

### 1. Cloner le dépôt
```bash
cd /Users/valorisa/Projets
git clone https://github.com/Alishahryar1/free-claude-code.git
```

### 2. Installer uv et Python 3.14
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
uv python install 3.14
```

### 3. Installer les dépendances
```bash
cd /Users/valorisa/Projets/free-claude-code
uv sync
```

## ⚙️ Configuration (à faire une seule fois)

### Fichier `.env`
Éditer `/Users/valorisa/Projets/free-claude-code/.env` :

```dotenv
# Clé API NVIDIA NIM (OBLIGATOIRE)
NVIDIA_NIM_API_KEY="nvapi-votre-clé"

# Modèles (optionnel mais recommandé)
MODEL_OPUS="nvidia_nim/moonshotai/kimi-k2"
MODEL_SONNET="nvidia_nim/z-ai/glm4.7"
MODEL_HAIKU="nvidia_nim/minimax/minimax-m1"
MODEL="nvidia_nim/z-ai/glm4.7"

# Token d'authentification (laisser tel quel)
ANTHROPIC_AUTH_TOKEN="freecc"
```

**Obtenir une clé NVIDIA NIM gratuite** : https://build.nvidia.com/settings/api-keys

## 🚀 Utilisation quotidienne

### Méthode 1 : Deux terminaux (recommandé)

**Terminal 1 - Lancer le proxy** :
```bash
cd /Users/valorisa/Projets/free-claude-code
uv run uvicorn server:app --host 0.0.0.0 --port 8082
```
→ Laissez ce terminal ouvert.

**Terminal 2 - Lancer Claude Code** :
```bash
ANTHROPIC_AUTH_TOKEN="freecc" ANTHROPIC_BASE_URL="http://localhost:8082" claude
```

### Méthode 2 : Un seul terminal
```bash
cd /Users/valorisa/Projets/free-claude-code
uv run uvicorn server:app --host 0.0.0.0 --port 8082 &
ANTHROPIC_AUTH_TOKEN="freecc" ANTHROPIC_BASE_URL="http://localhost:8082" claude
```
Pour arrêter le proxy plus tard : `kill %1` ou `pkill -f uvicorn`

## 💻 Intégration VSCode

L'extension Claude Code est déjà configurée dans VSCode (`settings.json`) :
```json
"claudeCode.environmentVariables": [
    { "name": "ANTHROPIC_BASE_URL", "value": "http://localhost:8082" },
    { "name": "ANTHROPIC_AUTH_TOKEN", "value": "freecc" }
]
```
→ Il suffit que le proxy tourne (Terminal 1) pour que l'extension fonctionne.

## 🧠 Choix des modèles NVIDIA NIM

| Modèle | Identifiant | Usage recommandé |
|--------|-------------|------------------|
| GLM-4.7 | `nvidia_nim/z-ai/glm4.7` | Équilibré (par défaut) |
| Kimi K2 | `nvidia_nim/moonshotai/kimi-k2` | Tâches complexes |
| MiniMax M1 | `nvidia_nim/minimax/minimax-m1` | Rapide, tâches simples |

**Note** : "Claude Opus 4.7" n'existe pas. Opus/Sonnet/Haiku sont des tiers de Claude Code qui sont routés vers les modèles NVIDIA NIM que vous configurez.

## 🔧 Dépannage

### Le proxy ne démarre pas
- Vérifiez que le fichier `.env` contient bien votre clé NVIDIA NIM
- Vérifiez que le port 8082 n'est pas déjà utilisé : `lsof -i :8082`

### Claude Code ne se connecte pas
- Vérifiez que le proxy tourne (Terminal 1)
- Vérifiez l'URL : `http://localhost:8082` (pas de `/v1` à la fin)

### Réinitialiser la configuration
```bash
cd /Users/valorisa/Projets/free-claude-code
cp .env.example .env
# Puis re-éditer avec votre clé
```

## 📝 Résumé des commandes essentielles

```bash
# Démarrer le proxy
cd /Users/valorisa/Projets/free-claude-code && uv run uvicorn server:app --host 0.0.0.0 --port 8082

# Démarrer Claude Code
ANTHROPIC_AUTH_TOKEN="freecc" ANTHROPIC_BASE_URL="http://localhost:8082" claude

# Vérifier les processus
ps aux | grep uvicorn
```

---
*Dernière mise à jour : Mai 2026*
