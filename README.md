# 🤖 Assistant Vocal K-2SO : Hub Central

Bienvenue dans le dépôt centralisé du projet **K-2SO**. Ce dépôt contient l'intégralité des composants pour transformer votre Home Assistant en un assistant vocal intelligent, contextuel et avec du caractère.

## 🏗️ Architecture du Système

Voici comment les données circulent entre votre voix et vos appareils :

```mermaid
graph TD
    A[🎤 Voix (Utilisateur)] -->|Commande| B(🛰️ Satellites ESPHome)
    B -->|Audio| C[🏠 HA Assist / Whisper]
    C -->|Texte| D{🎯 Matcher d'Intents}
    D -->|Pièce Détectée| E[🧠 Template : Satellite Mémorisé]
    E -->|Context| F[📜 Intent Scripts]
    F -->|Action| G[💡 Appareils / Lumières / Volets]
    F -->|Notification| H[📱 App HA / Persistent Notif]
```

## 🗺️ La Route vers l'Automatisation Totale

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

## 🚀 Comment démarrer ?

1.  **Exploration** : Lisez le README de la [Phase 1](./phase_1/README.md).
2.  **Préparation** : Préparez vos propres `entity_id` (Cibles & Satellites).
3.  **Déploiement** : Suivez les instructions "Express" dans l'ordre (1 -> 2 -> 2.1).

> [!CAUTION]
> **Adaptation obligatoire** : Vous DEVEZ remplacer les identifiants d'entités par les vôtres pour que le système soit opérationnel.

---
*Projet développé pour une immersion totale. Préparation pour la Phase 3 (IA & K-2SO)...* 🤖🚀
