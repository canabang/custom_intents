# 🧭 Phase 2 : Modularité & Contexte Multi-Pièces

Cette phase transforme votre base technique en un système **intelligent et contextuel**. Elle permet de piloter toute la maison sans jamais nommer les pièces.

## 📂 Structure Modulaire (Phore)

*   **[`intents/`](./intents/)** :
    *   `contextual_lights.yaml` : Lumières ("Allume la lumière").
    *   `contextual_covers.yaml` : Volets ("Ferme les volets").
    *   `shortcuts.yaml` : Raccourcis quotidiens (Café, Dodo).
    *   `intent_scripts.yaml` : Logique de détection dynamique (Pattern : `type_piece`).
*   **[`Templates/`](./Templates/)** :
    *   `satellite_actif_memorise.yaml` : Le cerveau qui mémorise quelle pièce a parlé.

## 🚀 Logique de Naming (CRITIQUE)
Le système repose sur une convention de nommage stricte. Pour que la détection automatique fonctionne, vos entités **doivent** suivre ce format ou un format similaire, à vous d'adapter en conséquence :

- **Lumières** : `light.hue_<piece>` (ex : `light.hue_salon`, `light.hue_cuisine`)
- **Volets** : `cover.vol<piece>` (ex : `cover.volsalon`, `cover.volcuisine`)

## 🛠️ Pré-requis : Création des Helpers (Global)
Pour que les commandes "maison" (globales) fonctionnent, vous devez créer deux groupes (helpers) dans Home Assistant :

1.  **`light.hue_all`** : Un groupe contenant **toutes** les lumières que vous voulez piloter via "Allume la maison".
2.  **`cover.volets`** : Un groupe contenant **tous** les volets de la maison.

> [!TIP]
> Allez dans **Paramètres > Appareils et services > Entrées (Helpers) > Créer une entrée > Groupe**.

## 🚀 Procédure de Déploiement

1.  **Sentences** : Copiez les 3 fichiers `.yaml` du dossier `intents/` (SAUF `intent_scripts.yaml`) vers :
    - `/share/speech-to-phrase/custom_sentences/fr/`
    - `/config/custom_sentences/fr/`
2.  **Configuration** :
    - Ajoutez le contenu de `intent_scripts.yaml` dans votre fichier `/config/intent_scripts.yaml`.
    - Ajoutez le contenu de `Templates/satellite_actif_memorise.yaml` dans votre fichier `/config/template.yaml` s'il n'est pas déjà présent.
3.  **Redémarrage** : Rechargez les Intents et Templates dans Home Assistant (ou redémarrez).

## 🧪 Tests Geek
Le système intègre des phrases de déclenchement à connotation geek pour plus de fun :

- 💡 **Lumières** : *"Lumos"*, *"Jaffa Kree"*, *"Engagez"*, *"Bravo six, passage au noir"*.
- 🏠 **Global** : *"Activation totale"*, *"Blackout"*, *"Mode furtif"*, *"Pleine puissance"*.
- 🪟 **Volets** : *"Que le jour se lève"*, *"Ouvrez l'iris"*, *"Levez les boucliers"*, *"Protocole Bunker"*.
- ☕ **Shortcuts** : *"Overclocking humain"*, *"Erreur de syntaxe : café requis"*, *"Entrée en stase"*.

---

**Félicitations ! Votre maison est maintenant contextuelle et prête pour la Phase 3 (IA & K-2SO).** 🤖💎
