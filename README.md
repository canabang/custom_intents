# 🤖 Assistant Vocal K-2SO : Hub Central

Qui n'a pas rêvé de commander sa maison à la voix, avec ses propres mots et expressions ? 
Baignant dans l'univers geek depuis toujours et fan inconditionnel de Star Trek, Star Wars, Stargate, Le Seigneur des Anneaux ou encore Doctor Who, mon but était simple : rendre ma maison aussi interactive que le pont de l'Enterprise ou la bibliothèque de Poudlard.

Pouvoir dire « Lumos » pour éclairer une pièce ou transformer son salon en « mode bunker » n'est plus de la science-fiction. À travers ces différentes phases, je vous invite à suivre mon aventure dans la création d'un assistant vocal vraiment personnel, intelligent et avec du caractère.

## 🏗️ Architecture du Système

Voici comment les données circulent entre votre voix et vos appareils :

```mermaid
graph TD
    A["🎤 Voix (Utilisateur)"] -->|Commande| B["🛰️ Satellites ESPHome"]
    B["🛰️ Satellites ESPHome"] -->|Audio| C["🏠 HA Assist / Speech-to-Phrase"]
    C -->|Texte| D{"🎯 Matcher d'Intents"}
    D -->|Pièce Détectée| E["🧠 Template : Satellite Mémorisé"]
    E -->|Contexte| F["📜 Intent Scripts"]
    F -->|Action| G["💡 Appareils / Lumières / Volets"]
    F -->|Notification| H["📱 App HA / Persistent Notif"]
```

> [!TIP]
> **Architecture Hybride** : Ce projet utilise une écoute 100% locale (ESP32) pour la fiabilité, mais peut utiliser le Cloud (Alexa) pour la sortie audio haute qualité. 
> [En savoir plus sur l'Architecture Vocale](./docs/architecture_vocale.md)

## 🗺️ La Route vers l'Automatisation Totale

### 📖 Concepts Fondamentaux
Avant de commencer, il est crucial de comprendre les piliers du projet :
- **[Conventions de Nommage](./docs/conventions_nommage.md)** : La règle d'or pour que le contexte fonctionne.
- **[Architecture Vocale](./docs/architecture_vocale.md)** : Pourquoi le local est roi.

---

| Phase | Nom | Focus | Fonctionnalité Clé |
| :--- | :--- | :--- | :--- |
| **Phase 1** | [Validation Technique](./phase_1/) | Fiabilité | Allumage direct et validation du flux. |
| **Phase 2** | [Modularité & Contexte](./phase_2/) | Pièces | Détection automatique de la pièce (Lumières & Volets). |
| **Phase 2.1** | [Intelligence Avancée](./phase_2.1/) | Scènes | Choix automatique des scènes (Jour/Nuit/Veilleuse). |
| **Phase 3** | **IA & Personnalité** | **Caractère** | **Intégration Gemini & Humour K-2SO.** |

---

### 🧪 [Phase 1 : Validation Technique Directe](./phase_1/)
**Objectif** : Valider le flux "Voix → Home Assistant" le plus vite possible.
- **Pourquoi la suivre ?** Pour être sûr que votre matériel est bien synchronisé.

### 🧭 [Phase 2 : Modularité & Contexte](./phase_2/)
**Objectif** : Rendre la maison consciente de votre position.
- **Pourquoi la suivre ?** Pour ne plus jamais avoir à nommer les pièces.

### 🧠 [Phase 2.1 : Intelligence d'Éclairage](./phase_2.1/)
**Objectif** : Gestion dynamique et scènes intelligentes.
- **Technique** : Intégration du script `gerer_eclairage`.

---

## 🛠️ Configuration Matérielle
Ce projet a été développé et testé avec les équipements suivants :
-  **Serveur Central** : BOX-3 (Home Assistant OS).
-  **Microphone Principal** : ReSpeaker Kit.
-  **Satellites de Zone** : 2 x Atom Echo (ESPHome).
-  **Sortie Audio** : Amazon Echo (Studio D, Show Cuisine/Chambre, SdB).

---

## 💻 Pré-requis Logiciels
Pour faire fonctionner ce projet, vous avez besoin de :
-  **Home Assistant** (Core ou OS).
-  **Speech-to-Phrase** (Add-on ou conteneur) : C'est le moteur qui transforme votre voix en textes reconnus localement sans passer par le cloud.
-  **ESPHome** : Pour la gestion de vos satellites (Atom Echo, ReSpeaker, etc.).

---

## 🚀 Comment démarrer ?

1.  **Exploration** : Lisez le README de la [Phase 1](./phase_1/README.md).
2.  **Préparation** : Préparez vos propres `entity_id` (Cibles & Satellites).
3.  **Déploiement** : Suivez les instructions "Express" dans l'ordre (1 -> 2 -> 2.1).

> [!CAUTION]
> **Adaptation obligatoire** : Vous DEVEZ remplacer les identifiants d'entités par les vôtres pour que le système soit opérationnel.

---
*Projet développé pour une immersion totale. Préparation pour la Phase 3 (IA & K-2SO)...* 🤖🚀
