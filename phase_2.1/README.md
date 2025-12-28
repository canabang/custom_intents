# 🧠 Phase 2.1 : Intelligence d'Éclairage Avancée

Cette phase est une évolution directe de la **Phase 2**. Elle conserve toute l'architecture modulaire et contextuelle, mais remplace l'allumage direct des lumières par un **script intelligent**.

## 🚀 Qu'est-ce qui change ?

Contrairement à la Phase 2 qui faisait un simple "ON/OFF", la Phase 2.1 utilise le script **`gerer_eclairage.yaml`** pour les commandes par pièce.

### Avantages :
- **Scènes Dynamiques** : Le système choisit automatiquement la scène (Veilleuse, Atténué, Stimulation) selon l'heure et l'état du soleil.
- **Sécurité** : Le script vérifie si vous êtes bien à la maison avant d'agir.
- **Blocages intelligents** : Évite d'allumer si la pièce est marquée comme "occupée" (ex: capteur de lit).

## 📂 Fichiers de cette Phase

*   **[`scripts/gerer_eclairage.yaml`](./scripts/gerer_eclairage.yaml)** : Le moteur d'intelligence d'éclairage.
*   **[`intents/intent_scripts.yaml`](./intents/intent_scripts.yaml)** : Logique mise à jour pour appeler le script (uniquement pour les pièces, le global reste direct).

> [!NOTE]
> Les fichiers de **Sentences** (phrases) et les **Templates** (détection satellite) sont strictement identiques à ceux de la **Phase 2**. Utilisez ceux du dossier Phase 2.

## 🚀 Déploiement

1.  **Script** : Importez le contenu de `gerer_eclairage.yaml` dans vos scripts HA.
2.  **Intents** : Utilisez le `intent_scripts.yaml` de ce dossier.
3.  **Sentences** : Reprenez les fichiers de la Phase 2.
4.  **Helpers** : Assurez-vous d'avoir bien vos groupes `light.hue_all` et `cover.volets`.

---

**Le socle technique est maintenant prêt pour une gestion "Pro" de vos lumières. Prochaine étape : K-2SO !** 🤖💎
