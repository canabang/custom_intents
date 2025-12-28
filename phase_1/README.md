# 🧪 Phase 1 : Validation Technique Directe

Cette phase est conçue pour valider votre installation vocale le plus rapidement possible, sans aucune complexité inutile. Elle teste le flux direct : **Voix → Satellite → Action**.

> [!WARNING]
> **ENTITÉS À ADAPTER :** Les noms d'entités utilisés dans ces fichiers (ex: `light.hue_salon`, `assist_satellite.esp_va_salon...`) sont des exemples basés sur ma propre installation. Vous **devez** les remplacer par vos propres Entity IDs dans les fichiers YAML pour que cela fonctionne chez vous.

## 📂 Structure Simplifiée

*   **[`intents/`](./intents/)** :
    *   `lumiere_salon.yaml` : Toutes les phrases (spécifiques et génériques).
    *   `intent_scripts.yaml` : Logique d'action directe et validation satellite.
*   **[`Templates/`](./Templates/)** :
    *   `satellite_actif_memorise.yaml` : Détection automatique du satellite qui écoute.
     Pourquoi ce template ? Je n'ai pas réussi a recuperer directement le nom du satellite qui écoute dans le trigger. Du coup avec ce template, je peux recuperer le nom du satellite qui écoute et le stocker dans une variable.
     
## 🚀 Procédure "Express" (2 minutes)

### 1. Lumières salon (Sentences)
Copiez les fichiers `.yaml` du dossier `intents/` (SAUF `intent_scripts.yaml`) vers :
- `/share/speech-to-phrase/custom_sentences/fr/`
- `/config/custom_sentences/fr/`
*Puis redémarrez l'add-on Speech-to-Phrase.*

### 2. Configuration
Ajoutez ces lignes dans votre `configuration.yaml` :

```yaml
intent_script: !include intent_scripts.yaml
template: !include template.yaml
```

1.  Copiez le contenu de `intents/intent_scripts.yaml` dans votre fichier `/config/intent_scripts.yaml`.
2.  Copiez le contenu de `Templates/satellite_actif_memorise.yaml` dans votre fichier `/config/template.yaml`.

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

---

**Une fois que ces deux tests réussissent, votre base technique est 100% validée. Vous êtes prêt pour la Phase 2 !** 🎯
