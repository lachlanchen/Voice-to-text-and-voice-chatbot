[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# Chatbot vocal-à-vocal avec Whisper, LLaMA et l'API Groq

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20gTTS-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)

Ce projet est un chatbot vocal en temps réel qui exploite Whisper d'OpenAI pour la transcription parole-texte, LLaMA 8B via l'API Groq pour générer des réponses, et Google Text-to-Speech (gTTS) pour reconvertir le texte en parole. L'interface du chatbot est construite avec Gradio, ce qui permet aux utilisateurs d'interagir avec le bot en parlant ou en téléversant des fichiers audio.

## Vue d'ensemble

L'application implémente une boucle complète de conversation audio dans un seul script Python :

1. Accepter l'audio utilisateur depuis un microphone ou un fichier téléversé.
2. Transcrire la parole en texte avec Whisper (modèle `base`).
3. Générer une réponse via Groq (`llama3-8b-8192`).
4. Reconvertir le texte de réponse en audio MP3 avec gTTS.
5. Renvoyer à la fois le texte de réponse et un audio lisible dans une interface web Gradio.

### Pipeline de conversation

| Étape | Composant | Sortie |
|---|---|---|
| 🎙️ Entrée | Audio Gradio (mic/fichier) | Chemin audio |
| 📝 Transcription | Whisper `base` | Texte utilisateur |
| 🧠 Raisonnement | Groq + `llama3-8b-8192` | Texte assistant |
| 🔊 Synthèse | gTTS | Réponse MP3 |
| 🖥️ Restitution | Interface Gradio | Texte + audio lisible |

## Fonctionnalités

- **Speech-to-Text** : Convertissez la parole en texte avec le modèle Whisper d'OpenAI.
- **Réponses générées par IA** : Utilisez LLaMA 8B via l'API Groq pour produire des réponses pertinentes à partir du texte transcrit.
- **Text-to-Speech** : Reconvertissez les réponses textuelles en parole via Google Text-to-Speech (gTTS).
- **Interaction en temps réel** : Le chatbot fonctionne en temps réel, permettant d'interagir via microphone ou en téléversant des fichiers audio sur une interface web.
- **Exécution simple sur un seul fichier** : Toute la chaîne du chatbot est implémentée dans `voice_to_voice_chatbot.py` pour faciliter l'expérimentation.

## Structure du projet

```text
Voice-to-text-and-voice-chatbot/
├── voice_to_voice_chatbot.py               # Script principal pour exécuter le chatbot
├── requirements.txt                        # Liste des dépendances Python
├── README.md                               # Documentation du projet (anglais)
├── i18n/                                   # README localisés (prévus/générés dans le pipeline)
└── .auto-readme-work/20260228_230442/      # Contexte/artefacts d'automatisation du README
```

Référence de structure héritée depuis le README canonique :

```text
voice-to-voice-chatbot/
├── voice_to_voice_chatbot.py  # Script principal pour exécuter le chatbot
├── requirements.txt           # Liste des dépendances Python
├── README.md                  # Documentation du projet
└── .gitignore                 # Fichier d'exclusion Git
```

## Prérequis

Avant de commencer, assurez-vous d'avoir les éléments suivants :

- Python 3.7 ou supérieur installé sur votre machine locale ou sur Google Colab.
- Une clé API Groq. Vous pouvez en obtenir une [ici](https://groq.com/).
- [Google Colab](https://colab.research.google.com/) ou un environnement Python local avec les bibliothèques nécessaires installées.
- Un accès Internet pour :
  - Le téléchargement initial du modèle Whisper.
  - Les appels API Groq.
  - La génération audio gTTS.

### Exigences en un coup d'oeil

| Exigence | Pourquoi c'est nécessaire |
|---|---|
| Python `3.7+` | Environnement d'exécution pour le script du chatbot et ses dépendances |
| Clé API Groq | Accès authentifié à l'inférence du modèle LLaMA |
| Colab ou environnement local | Environnement d'exécution pour Gradio + bibliothèques ML |
| Accès Internet | Téléchargement du modèle Whisper, requêtes Groq, synthèse gTTS |

## Installation

Suivez ces étapes pour configurer le projet :

1. **Cloner le dépôt :**

```bash
git clone https://github.com/aquibali01/voice-to-voice-chatbot.git
cd voice-to-voice-chatbot
```

2. **Installer les dépendances :**

Installez les bibliothèques Python requises :

```bash
pip install -r requirements.txt
```

Sinon, si vous utilisez Google Colab, vous pouvez installer les bibliothèques avec :

```python
!pip install gradio groq-api openai-whisper gtts
```

## Configuration

### Configurer la clé API Groq

Ajoutez votre clé API Groq aux variables d'environnement :

```bash
export GROQ_API_KEY='your_groq_api_key'
```

Dans Google Colab, vous pouvez définir la clé API avec :

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### Remarque importante d'exécution (comportement actuel du code)

Le script actuel initialise le client Groq avec un placeholder codé en dur :

```python
client = Groq(api_key="your_groq_api_key")
```

Si vous définissez uniquement `GROQ_API_KEY` dans l'environnement, mettez à jour le script pour lire `os.environ` ou remplacez directement le placeholder, sinon l'authentification API échouera.

## Utilisation

Pour démarrer le chatbot, exécutez le script principal :

```bash
python voice_to_voice_chatbot.py
```

Ou dans Google Colab :

Copiez le script dans une cellule de code et exécutez-la.

L'interface Gradio se lancera et vous permettra d'interagir avec le chatbot.

### Interaction avec le chatbot

- **Avec le microphone** : Parlez directement dans le microphone. Le chatbot transcrira votre voix, générera une réponse et la relira sous forme audio.
- **Téléversement d'audio** : Téléversez un fichier audio préenregistré. Le chatbot transcrira l'audio, générera une réponse et reconvertira la réponse en parole.

## Exemples

### Exemple de flux vocal

1. Enregistrer : "What are three tips to learn Python quickly?"
2. Whisper transcrit votre texte de prompt.
3. Le modèle LLaMA de Groq génère une réponse.
4. gTTS produit une réponse MP3.
5. Gradio affiche le texte de réponse et la lecture audio.

### Exemple de commande d'exécution

```bash
python voice_to_voice_chatbot.py
```

Résultat attendu : une application Gradio locale s'ouvre dans votre navigateur avec une entrée audio et deux sorties (texte + audio).

## Notes de développement

- Fonction principale du pipeline : `chatbot_pipeline(audio_path)`.
- Le modèle Whisper est chargé au démarrage : `whisper.load_model("base")`.
- Des fichiers MP3 temporaires sont créés via `NamedTemporaryFile(..., delete=False)`.
- La gestion des erreurs renvoie actuellement `(str(e), None)` vers l'interface.
- Les dépendances dans `requirements.txt` incluent `whisper` et `openai-whisper` ; cela peut être redondant selon l'environnement.

## Dépannage

### Problèmes courants

`ModuleNotFoundError` : assurez-vous d'avoir installé la bonne version du module Whisper avec

```python
!pip install -U openai-whisper
```

`Groq API Key Error` : vérifiez votre clé API et assurez-vous qu'elle est correctement définie dans les variables d'environnement.

Dépannage supplémentaire :

- Si l'application échoue immédiatement avec des erreurs d'authentification, vérifiez la valeur codée en dur `api_key="your_groq_api_key"` dans `voice_to_voice_chatbot.py`.
- Si la capture micro n'est pas disponible, téléversez d'abord un fichier audio pour valider la chaîne STT -> LLM -> TTS.
- Si l'audio de réponse est vide, confirmez que l'accès réseau sortant est disponible pour gTTS.

### Checklist de diagnostic rapide

| Vérification | Validation |
|---|---|
| Câblage de la clé API | `Groq(api_key=...)` n'est pas laissé avec le placeholder |
| Installation Whisper | `openai-whisper` s'importe correctement |
| Chemin réseau | Accès sortant disponible pour Groq + gTTS |
| Source audio | Permissions micro activées ou téléversement de fichier opérationnel |

## Feuille de route

- Lire la clé API Groq directement depuis les variables d'environnement par défaut.
- Ajouter des tests pour les fonctions utilitaires du pipeline.
- Ajouter des paramètres optionnels de modèle/configuration via variables d'environnement ou arguments CLI.
- Ajouter des fichiers README i18n sous `i18n/` correspondant aux liens de navigation de langue.
- Ajouter des options de déploiement (par exemple Docker ou Hugging Face Spaces).

## Contribution

Les contributions sont les bienvenues ! Veuillez forker ce dépôt, créer une nouvelle branche et soumettre une pull request avec vos modifications.

Flux de contribution suggéré :

1. Forker et cloner le dépôt.
2. Créer une branche de fonctionnalité.
3. Effectuer et tester vos modifications.
4. Ouvrir une pull request avec une description claire.

## Support

Aucun lien explicite de don/sponsoring n'a été trouvé dans le contenu actuel du dépôt. Si les mainteneurs ajoutent des canaux de support, ils devraient être listés ici.

## Licence

Ce projet est sous licence MIT. Consultez le fichier LICENSE pour plus de détails.

Hypothèse : un fichier `LICENSE` est prévu, mais peut ne pas encore être présent dans cet instantané du dépôt.
