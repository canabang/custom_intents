# 🧭 Phase 2 : Modularité & Contexte Multi-Pièces

Cette phase transforme votre base technique en un système **intelligent et contextuel**. Elle permet de piloter toute la maison sans jamais nommer les pièces.

## 📂 Structure Modulaire

*   **[`intents/`](./intents/)** :
    *   `contextual_lights.yaml` : Lumières ("Allume la lumière").
    *   `contextual_covers.yaml` : Volets ("Ferme les volets").
    *   `shortcuts.yaml` : Raccourcis quotidiens (Café, Dodo).
    *   `intent_scripts.yaml` : Logique de détection dynamique (Pattern : `type_piece`).
*   **[`Templates/`](./Templates/)** :
    *   `satellite_actif_memorise.yaml` : Le cerveau qui mémorise quelle pièce a parlé.

## 🚀 Logique de Naming (CRITIQUE)
Le système repose sur une convention de nommage stricte. Pour que la détection automatique fonctionne, vos entités **doivent** suivre ce format ou un format similaire , à vous d'adapter en conséquence :

- **Lumières** : `light.hue_<piece>` (ex : `light.hue_salon`, `light.hue_cuisine`)
- **Volets** : `cover.vol<piece>` (ex : `cover.volsalon`, `cover.volcuisine`)

## 🚀 Procédure de Déploiement

1.  **Sentences** : Copiez les 3 fichiers `.yaml` du dossier `intents/` (SAUF `intent_scripts.yaml`) vers :
    - `/share/speech-to-phrase/custom_sentences/fr/`
    - `/config/custom_sentences/fr/`
2.  **Configuration** :
    - Ajoutez le contenu de `intent_scripts.yaml` dans votre fichier `/config/intent_scripts.yaml`.
    - Ajoutez le contenu de `Templates/satellite_actif_memorise.yaml` dans votre fichier `/config/template.yaml` si il n'est pas déjà présent.
3.  **Redémarrage** : Rechargez les Intents et Templates dans Home Assistant (ou redémarrez).

## 🧪 Tests de Validation

### Test 1 : Lumière Contextuelle
Allez dans la **Cuisine** et dites : *"Allume la lumière"*.
- ✅ `light.hue_cuisine` s'allume.
- ✅ Une notification HA confirme : "Pièce : cuisine".

### Test 2 : Volet Contextuel
Allez dans le **Chambre** et dites : *"Ferme le volet"*.
- ✅ `cover.volchambre` se ferme.
- ✅ Une notification HA confirme : "Pièce : chambre".

---

**Félicitations ! Votre maison est maintenant contextuelle. Prochaine étape : Phase 3 (Personnalité IA & Voix avec K-2SO).** 🤖💎
