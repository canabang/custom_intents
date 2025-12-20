# 🎓 Tutoriel : Donnez vie et caractère à votre Home Assistant (Projet K-2SO)

Bienvenue dans ce guide complet pour implémenter **K-2SO**, un assistant vocal contextuel avec une personnalité sarcastique unique. Ce tutoriel vous guidera de la configuration de base à une intelligence domotique capable de vous localiser et d'agir sans que vous ayez à préciser "où" vous êtes.

---

## 🏗️ Étape 0 : Matériel et Prérequis

Pour une expérience K-2SO optimale, ce projet s'appuie sur une architecture matérielle spécifique :

### 🎙️ Assistant Vocal (Saisie)
*   **1 x ESP32-S3-BOX-3** : Salon.
*   **1 x ReSpeaker Kit** : Chambre.
*   **2 x M5Stack Atom Echo** : Cuisine et Salle de Bain.

### � Détection de Présence (ESPHome)
*   **4 x LD2410C** : Un par pièce (Salon, Cuisine, Chambre, SdB) pour la détection de présence radar.
*   **1 x LD2450** : Salon (détection multi-zones avancée).
*   Chaque capteur est monté sur un ESP32 et tourne sous ESPHome.

### �🔊 Enceintes Amazon Echo (Sortie)
K-2SO utilise vos enceintes existantes pour vous répondre :
*   **Echo Studio D** (Salon)
*   **Echo Show Cuisine**
*   **Echo Show Chambre**
*   **Echo SdB**
*   **Echo Dot Gael** (Point de secours / Global)

### 📚 Prérequis Logiciel
Ce projet s'appuie sur plusieurs composants logiciels essentiels :

#### Reconnaissance et Synthèse Vocale
*   **STT (Speech-to-Text)** : [Speech-to-Phrase](https://github.com/OHF-Voice/speech-to-phrase) - Add-on Home Assistant pour la reconnaissance vocale locale ultra-rapide.
    *   Utilise Kaldi + FST (Finite State Transducer) pour une reconnaissance optimisée.
    *   Entraînement automatique basé sur vos entités Home Assistant.
    *   **Configuration critique** : 
        - Copier les fichiers de sentences dans `/share/speech-to-phrase/custom_sentences/fr/` ET `/config/custom_sentences/fr/`
        - Redémarrer l'add-on après chaque modification pour réentraîner le modèle
        - Vérifier les logs de l'add-on en cas d'échec de reconnaissance
*   **TTS (Text-to-Speech)** : [Piper](https://github.com/home-assistant/addons/tree/master/piper) - Add-on Home Assistant officiel pour la synthèse vocale naturelle en français.

#### Intelligence Artificielle
*   **Google Gemini AI** : Utilisé pour générer les réponses sarcastiques de K-2SO via le service `ai_task.generate_data`.
*   **Fallback Intelligent** : Si l'IA est indisponible (quota dépassé), K-2SO bascule automatiquement sur des phrases pré-écrites aléatoires.

#### Détection de Présence
Ce projet s'appuie directement sur la méthodologie que j'ai co-écrite avec **freetronic** sur HACF. C'est le socle qui permet à Home Assistant de savoir dans quelle pièce vous vous trouvez et quelle enceinte Echo doit vous répondre.

> [!IMPORTANT]
> **Article prérequis :** [Notifications dynamiques par pièce occupée (HACF)](https://www.hacf.fr/notifications-dynamiques-par-piece/)
>
> Vous **devez** avoir implémenté les éléments suivants de cet article :
> 1. Le **Sensor de présence** (qui définit la pièce active et l'enceinte Echo associée).
> 2. Les **Scripts de notification** (Alexa/Spotify).

> [!WARNING]
> **Personnalisation Obligatoire :** Les fichiers de ce projet contiennent des noms d'entités spécifiques à mon installation :
> - `media_player.echo_studio_d`, `media_player.echo_show_cuisine`, etc.
> - `binary_sensor.esp_cuisine_presence`, `binary_sensor.salon_presence_globale`, etc.
> - `light.hue_salon`, `cover.volsalon`, `switch.priscafe`, etc.
>
> **Vous devez impérativement adapter ces noms** pour qu'ils correspondent à vos propres entités Home Assistant avant le déploiement !

Une fois que votre Home Assistant sait vous localiser et vous répondre dynamiquement, vous êtes prêt pour la suite !

---

## 🧪 Étape 1 : Phase 1 - Validation Technique (Le "Bac à Sable")

L'objectif ici est de tester votre micro, votre reconnaissance vocale (STT) et la "voix" de K-2SO dans une seule pièce : le **Salon**.

### 1.1 Installation des Sentences
Copiez les fichiers du dossier `phase_1/intents/` (hors `intent_scripts.yaml`) vers :
- `/share/speech-to-phrase/custom_sentences/fr/` (pour l'entraînement vocal)
- `/config/custom_sentences/fr/` (pour Home Assistant)

### 1.2 Configuration des Scripts
Allez dans l'interface UI de Home Assistant (**Paramètres > Automatisations > Scripts**) et créez les 3 scripts présents dans `phase_1/scripts/` en copiant leur contenu YAML.

### 1.3 Inclusion
Ajoutez le contenu de `phase_1/intents/intent_scripts.yaml` dans votre configuration et déclarez votre sensor de présence basic.

**Test** : Dites *"Lumos"* ou *"Allume le salon"*. K-2SO doit vous confirmer l'action avec sarcasme.

---

## 🤖 Étape 2 : Phase 2 - L'Intelligence Contextuelle Finale

Maintenant que la technique est validée, on passe au niveau supérieur : **vous ne nommez plus les pièces**.

### 2.1 Pourquoi la Phase 2 ?
Dans cette phase, si vous dites *"Ferme le volet"* depuis la cuisine, c'est le volet de la cuisine qui descend. K-2SO utilise votre position réelle pour deviner votre intention.

### 2.2 Déploiement "Pro"
1. **Sentences** : Déployez les fichiers `contextual_*.yaml` et `shortcuts.yaml` du dossier `phase_2/intents/`.
2. **Template Pro** : Utilisez le fichier `presence_piece.yaml` (Phase 2) qui gère les conflits de présence et automatise le ciblage des enceintes.
3. **Mise à jour des Scripts** : Remplacez vos scripts UI par les versions de la Phase 2 pour activer la gestion globale.

---

## 🧠 Philosophie et Maintenance

- **DRY (Don't Repeat Yourself)** : Toute la logique complexe est dans les **Scripts**. Vos commandes vocales (Intents) restent ainsi très simples et rapides à lire.
- **Caractère** : Le script `k_2so_confirm_action` utilise Google Gemini pour varier ses réponses. Si l'IA est hors-ligne, une liste de sarcasmes de secours prend le relais.
- **Évolutivité** : Vous voulez ajouter une nouvelle pièce ? Il suffit d'ajouter son nom dans votre sensor de présence et de nommer vos équipements de manière standardisée (ex: `light.hue_cuisine`).

---

*Ce tutoriel est conçu pour vous offrir une immersion totale. Merci à la communauté HACF et à freetronic pour les bases solides !* 🤖🚀
