# 🤖 Custom Intents : Projet K-2SO

Bienvenue dans le dépôt centralisé du projet **K-2SO**. Ce dépôt contient l'intégralité des composants nécessaires pour transformer votre Home Assistant en un assistant vocal intelligent, contextuel et avec du caractère.

## 📂 Structure du Dépôt

L'architecture est modulaire pour faciliter le déploiement et la maintenance :

*   **[Intents](./intents/)** : Reconnaissance vocale (sentences) et logique d'exécution (intent scripts). Divisé en Phase 1 (Test) et Phase 2 (Production).
*   **[Scripts](./scripts/)** : Moteurs d'exécution centraux (Gestion d'éclairage intelligente, Notifications Alexa dynamiques).
*   **[Templates](./Templates/)** : Capteurs virtuels pour la détection de présence et le ciblage des enceintes Echo.

## 🚀 Démarrage Rapide

1.  **Exploration** : Commencez par lire le [README des Intents](./intents/README.md) pour comprendre la logique de déploiement.
2.  **Validation** : Déployez la **Phase 1** pour valider votre installation micro/STT dans une pièce simple.
3.  **Intelligence** : Une fois validé, passez à la **Phase 2** pour activer la détection contextuelle dans toute la maison.

## 🧠 Philosophie du Projet

Ce projet repose sur le principe **DRY** (*Don't Repeat Yourself*) : la logique lourde est centralisée dans des scripts réutilisables, laissant les commandes vocales courtes, propres et faciles à maintenir.

---
*Projet développé pour une immersion totale dans l'univers domotique. Bonne configuration !* 🤖🚀
