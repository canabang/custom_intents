# 🧪 Phase 1 : Validation Technique Directe

Cette phase est conçue pour valider votre installation vocale le plus rapidement possible, sans aucune complexité inutile. Elle teste le flux direct : **Voix → Satellite → Action**.
Elle permet aussi de comprendre le fonctionnement de base de l'assistant vocal, des `intent_scripts.yaml` et des `custom_sentences`.

> [!WARNING]
> **ENTITÉS À ADAPTER :** Les noms d'entités utilisés dans ces fichiers (ex: `light.hue_salon`, `assist_satellite.esp_va_salon...`) sont des exemples basés sur ma propre installation. Vous **devez** les remplacer par vos propres Entity IDs dans les fichiers YAML pour que cela fonctionne chez vous.

## 📂 Structure Simplifiée

*   **[`intents/`](./intents/)** :
    *   `lumiere_salon.yaml` : Toutes les phrases (spécifiques et génériques).
    *   `intent_scripts.yaml` : Logique d'action directe et validation satellite.
*   **[`Templates/`](./Templates/)** :
    *   `satellite_actif_memorise.yaml` : Détection automatique du satellite qui écoute.
     Pourquoi ce template ? Je n'ai pas réussi à récupérer directement le nom du satellite qui écoute dans le trigger. Du coup avec ce template, je peux récupérer le nom du satellite qui écoute et le stocker dans une variable.
     
## 🚀 Procédure "Express" (2 minutes)

### 1. Lumières salon (Sentences)
Copiez le fichier `lumiere_salon.yaml` du dossier `intents/` vers :
- `/share/speech-to-phrase/custom_sentences/fr/`
- `/config/custom_sentences/fr/`
*Puis redémarrez l'add-on Speech-to-Phrase.*

### 2. Configuration
Ajoutez ces lignes dans votre `configuration.yaml` si elles ne sont pas déjà présentes :

```yaml
intent_script: !include intent_scripts.yaml
template: !include template.yaml
```

1.  Copiez le contenu de `intents/intent_scripts.yaml` dans votre fichier `/config/intent_scripts.yaml` à créer si nécessaire.
2.  Copiez le contenu de `Templates/satellite_actif_memorise.yaml` dans votre fichier `/config/template.yaml` à créer si nécessaire.

### 3. Redémarrage
Redémarrez Home Assistant (ou rechargez les "Intents" et les "Templates").

> [!IMPORTANT]
> Avant de tester, assurez-vous d'avoir bien ouvert les fichiers YAML et remplacé les `entity_id` par les vôtres (surtout dans `intent_scripts.yaml` et `satellite_actif_memorise.yaml`).

## 🧪 Tests de Validation

### Test A : Le Micro Fonctionne
Dites : **"Banane"**.
- ✅ La lumière du **Salon** s'allume.
- ✅ Une notification HA confirme l'ordre.

### Test B : Le Satellite est Détecté
Allez dans une pièce (ex: Cuisine) et dites : **"Allume la lumière"**.
- ✅ La lumière du **Salon** s'allume (cible fixe pour Phase 1).
- ✅ La notification HA doit indiquer : **"Allumé via satellite : cuisine"**.

## ⏩ Limites & Transition vers la Phase 2

**"Tout ça c'est bien gentil, mais si je veux que ça fonctionne pour TOUTES les pièces ?"**

C'est là que l'on touche aux limites de la **Phase 1** :
- **Cible Fixe** : Actuellement, peu importe qui parle, c'est uniquement le salon qui réagit. 
- **Conflits de Phrases** : Si vous vouliez ajouter la cuisine "à la main" en créant un fichier par pièce, vous seriez tenté de réutiliser les mêmes phrases (*"allume la lumière"*). Mais Home Assistant ne peut pas savoir dans quelle pièce vous êtes sans une logique contextuelle.

**La solution ? La Phase 2 et son Intelligence Contextuelle.** 🧭

En passant à la Phase 2, nous allons :
1. **Supprimer les cibles fixes** pour utiliser des variables dynamiques : `light.hue_{{ piece }}`.
2. **Utiliser le Satellite** pour remplir cette variable automatiquement selon l'endroit où vous êtes.

> [!TIP]
> **Le bénéfice :** Vous pourrez dire *"Lumos"* ou *"Allume la lumière"* dans n'importe quelle pièce, et seule la lumière de **cette** pièce s'allumera. C'est ça, la vraie magie du contexte !

**[Prêt pour la suite ? Direction la Phase 2 !](../phase_2/)** 🚀