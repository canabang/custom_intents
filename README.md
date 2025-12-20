# 🤖 Custom Intents : Projet K-2SO

Bienvenue dans le dépôt centralisé du projet **K-2SO**. Ce dépôt contient l'intégralité des composants nécessaires pour transformer votre Home Assistant en un assistant vocal intelligent, contextuel et avec du caractère.

> [!NOTE]
> **Configuration Matérielle Déployée :** 1 x BOX-3, 1 x ReSpeaker Kit, 2 x Atom Echo, 4 x LD2410C + 1 x LD2450 (ESPHome), et un parc d'enceintes Amazon Echo (Studio D, Show Cuisine/Chambre, SdB).

## 📂 Structure du Dépôt

L'architecture est modulaire pour faciliter le déploiement et la maintenance :

*   **[Phase 1 : Test](./phase_1/)** : Validation technique isolée pour le salon (Sentences + Scripts + Template Basic).
*   **[Phase 2 : Production](./phase_2/)** : Déploiement complet et contextuel pour toute la maison (Sentences + Scripts + Template Pro).
*   **[_common.yaml](./_common.yaml)** : Listes de mots partagées pour les sentences vocales.

## 🚀 Démarrage Rapide

> [!TIP]
> **Nouveau :** Suivez notre **[Tutoriel Complet de A à Z](./TUTORIAL.md)** pour une installation pas à pas !

1.  **Exploration** : Commencez par lire le [README de la Phase 1](./phase_1/README.md) pour valider votre installation technique.
2.  **Validation** : Validez votre base technique avec la **Phase 1**.
3.  **Intelligence** : Passez à la **Phase 2** pour une maison contextuelle.

## 🧠 Philosophie du Projet

Ce projet repose sur le principe **DRY** (*Don't Repeat Yourself*) : la logique lourde est centralisée dans des scripts réutilisables, laissant les commandes vocales courtes, propres et faciles à maintenir.

> [!WARNING]
> **Personnalisation Requise :** Les fichiers contiennent des noms d'entités spécifiques à mon installation (Echo, capteurs, lumières, etc.). Vous devez adapter ces noms pour qu'ils correspondent à vos propres entités Home Assistant.

---
*Projet développé pour une immersion totale dans l'univers domotique. Bonne configuration !* 🤖🚀
