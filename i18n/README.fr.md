[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# Chatbot vocal-to-vocal avec Whisper, LLaMA et l'API Groq

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20gTTS-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)

Ce projet est un chatbot vocal en temps réel qui exploite Whisper d'OpenAI pour la transcription de la parole en texte, LLaMA 3 8B via l'API Groq pour générer les réponses, et Google Text-to-Speech (gTTS) pour reconvertir le texte en parole. L'interface utilise Gradio afin que les utilisateurs puissent interagir en parlant ou en téléversant des fichiers audio.

## Présentation

L'application implémente une boucle de conversation audio complète dans un seul script Python :

1. Accepter l'audio utilisateur depuis un microphone ou un fichier uploadé.
2. Transcrire la parole en texte avec le modèle Whisper (`base`).
3. Générer une réponse via Groq (`llama3-8b-8192`).
4. Reconvertir le texte de réponse en MP3 via gTTS.
5. Retourner à la fois le texte de réponse et l'audio jouable dans une interface web Gradio.

### Pipeline de conversation

| Étape | Composant | Sortie |
|---|---|---|
| 🎙️ Entrée | Gradio Audio (mic/fichier) | Chemin audio |
| 📝 Transcription | Whisper `base` | Texte utilisateur |
| 🧠 Raisonnement | Groq + `llama3-8b-8192` | Texte de l'assistant |
| 🔊 Synthèse | gTTS | Réponse MP3 |
| 🖥️ Livraison | Gradio UI | Texte + audio lisible |

## ⭐ Fonctionnalités

- **Speech-to-Text** : Convertir le langage parlé en texte avec le modèle Whisper d'OpenAI.
- **Réponses générées par IA** : Utiliser LLaMA 8B via l'API Groq pour générer des réponses intelligentes à partir du texte transcrit.
- **Text-to-Speech** : Reconvertir le texte de réponse en parole via Google Text-to-Speech (gTTS).
- **Interaction en temps réel** : Interagir via microphone ou en téléchargeant des fichiers audio via l'interface web.
- **Runtime simple en un seul fichier** : Le pipeline complet du chatbot est implémenté dans `voice_to_voice_chatbot.py`.
- **Documentation multilingue** : Le README principal est reproduit via des liens vers des traductions localisées dans `i18n/`.

## 📁 Structure du projet

```text
Voice-to-text-and-voice-chatbot/
├── README.md                     # Documentation principale du projet (anglais)
├── requirements.txt              # Dépendances Python
├── voice_to_voice_chatbot.py     # Script principal pour exécuter le chatbot
├── i18n/                        # README localisés
│   ├── README.ar.md
│   ├── README.de.md
│   ├── README.es.md
│   ├── README.fr.md
│   ├── README.ja.md
│   ├── README.ko.md
│   ├── README.ru.md
│   ├── README.vi.md
│   ├── README.zh-Hans.md
│   └── README.zh-Hant.md
└── .auto-readme-work/
    ├── 20260228_230442/
    └── 20260301_064403/
```

## ✅ Prérequis

Avant de commencer, assurez-vous d'avoir rempli les conditions suivantes :

- Python 3.7 ou supérieur installé sur votre machine locale ou Google Colab.
- Une clé API Groq. Vous pouvez vous inscrire pour obtenir une clé API à [Groq](https://groq.com/).
- [Google Colab](https://colab.research.google.com/) ou un environnement Python local avec les bibliothèques requises.
- Un accès Internet pour :
  - Le téléchargement initial du modèle Whisper.
  - Les appels à l'API Groq.
  - La génération audio avec gTTS.

### Exigences en un coup d'œil

| Exigence | Pourquoi c'est nécessaire |
|---|---|
| Python `3.7+` | Runtime du script du chatbot et de ses dépendances |
| Clé API Groq | Accès authentifié à l'inférence du modèle LLaMA |
| Colab ou environnement local | Environnement d'exécution pour Gradio + bibliothèques ML |
| Accès Internet | Téléchargement du modèle Whisper, requêtes Groq, synthèse gTTS |

## 🛠️ Installation

Suivez ces étapes pour configurer le projet :

1. **Cloner le dépôt**

```bash
git clone https://github.com/aquibali01/voice-to-voice-chatbot.git
cd voice-to-voice-chatbot
```

2. **Installer les dépendances**

Installez les bibliothèques Python requises :

```bash
pip install -r requirements.txt
```

Alternativement, dans Google Colab :

```python
!pip install gradio groq-api openai-whisper gtts
```

## ⚙️ Configuration

### Configurer la clé API Groq

Exportez votre clé API Groq :

```bash
export GROQ_API_KEY='your_groq_api_key'
```

Dans Google Colab, définissez-la pendant l'exécution :

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### Remarque importante sur le runtime actuel (comportement du code)

Le script actuel initialise le client Groq avec un placeholder codé en dur :

```python
client = Groq(api_key="your_groq_api_key")
```

Si vous ne définissez `GROQ_API_KEY` que dans l'environnement, mettez à jour le script pour lire `os.environ` (ou remplacez directement le placeholder), sinon les appels d'authentification peuvent échouer.

## ▶️ Utilisation

Pour démarrer le chatbot, exécutez :

```bash
python voice_to_voice_chatbot.py
```

Ou dans Google Colab :

Copiez le script dans une cellule de code et exécutez-le.

L'interface Gradio se lancera localement, vous permettant d'interagir avec le chatbot.

### Interagir avec le chatbot

- **Utiliser le micro** : Parlez directement dans le microphone. Le chatbot transcrit votre voix, génère une réponse et la lit en audio.
- **Télécharger un audio** : Téléversez un fichier audio préenregistré. Le chatbot transcrit l'enregistrement, génère une réponse et joue l'audio synthétisé.

## 🎬 Exemples

### Exemple de flux vocal

1. Enregistrement : "What are three tips to learn Python quickly?"
2. Whisper transcrit votre texte de prompt.
3. Le modèle Groq LLaMA génère une réponse.
4. gTTS produit une réponse MP3.
5. Gradio affiche le texte de réponse et un lecteur audio.

### Exemple de commande d'exécution

```bash
python voice_to_voice_chatbot.py
```

Résultat attendu : une app Gradio locale s'ouvre dans votre navigateur avec une entrée audio et deux sorties (texte + audio).

## 🧪 Notes de développement

- Fonction principale du pipeline : `chatbot_pipeline(audio_path)`.
- Le modèle Whisper est chargé au démarrage avec `whisper.load_model("base")`.
- Les fichiers MP3 temporaires sont créés avec `NamedTemporaryFile(..., delete=False)`.
- La gestion des erreurs renvoie actuellement `(str(e), None)` vers l'UI.
- `requirements.txt` inclut à la fois `whisper` et `openai-whisper` ; cela peut être redondant selon l'environnement.

## 🐞 Dépannage

### Problèmes courants

`ModuleNotFoundError` : assurez-vous d'installer le bon package Whisper :

```python
!pip install -U openai-whisper
```

`Groq API Key Error` : vérifiez la disponibilité et la validité de la clé dans votre environnement ou votre script.

Dépannage supplémentaire :

- Si l'application échoue avec des erreurs d'authentification, vérifiez la valeur codée en dur `api_key="your_groq_api_key"` dans `voice_to_voice_chatbot.py`.
- Si la capture du microphone n'est pas disponible, téléchargez d'abord un fichier audio pour valider le pipeline STT → LLM → TTS.
- Si l'audio de réponse est vide, confirmez l'accès réseau sortant pour gTTS et Groq.

### Liste de contrôle de diagnostic rapide

| Vérification | Validation |
|---|---|
| Branchement de la clé API | `Groq(api_key=...)` n'est pas laissé comme placeholder |
| Installation de Whisper | `openai-whisper` s'importe correctement |
| Chemin réseau | L'accès sortant est disponible pour Groq + gTTS |
| Source audio | Permissions micro activées ou upload de fichier opérationnel |

## 🗺️ Feuille de route

- Lire la clé API Groq directement depuis les variables d'environnement par défaut.
- Ajouter des tests pour les fonctions utilitaires du pipeline.
- Ajouter des paramètres optionnels de modèle/config via variables d'environnement ou arguments CLI.
- Ajouter des options de déploiement (par exemple, Docker ou Hugging Face Spaces).

## ❤️ Support

| Donate | PayPal | Stripe |
|---|---|---|
| [![Donate](https://img.shields.io/badge/Donate-LazyingArt-0EA5E9?style=for-the-badge&logo=ko-fi&logoColor=white)](https://chat.lazying.art/donate) | [![PayPal](https://img.shields.io/badge/PayPal-RongzhouChen-00457C?style=for-the-badge&logo=paypal&logoColor=white)](https://paypal.me/RongzhouChen) | [![Stripe](https://img.shields.io/badge/Stripe-Donate-635BFF?style=for-the-badge&logo=stripe&logoColor=white)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## 🤝 Contribution

Les contributions sont les bienvenues. Forkez ce dépôt, créez une nouvelle branche, et soumettez une pull request avec vos modifications.

Flux de contribution suggéré :

1. Forker et cloner le dépôt.
2. Créer une branche de fonctionnalité.
3. Implémenter et tester vos modifications.
4. Ouvrir une pull request avec une description claire et une justification.

## 📄 Licence

Ce projet est actuellement documenté comme étant sous licence MIT. Voir le fichier LICENSE pour plus de détails.

Hypothèse : un fichier `LICENSE` est prévu mais peut ne pas être encore présent dans cette version du dépôt.
