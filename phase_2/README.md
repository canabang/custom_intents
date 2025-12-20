# 🤖 Phase 2 : Intelligence Contextuelle (Production)

Cette phase est le déploiement complet et intelligent du projet **K-2SO**. Elle permet d'automatiser toute la maison (Lumières, Volets, Raccourcis) sans spécifier la pièce, en se basant uniquement sur votre position réelle.

Elle est totalement **autonome** et regroupe tous les fichiers nécessaires pour fonctionner de manière isolée.

## 📂 Structure du Dossier

*   **[`intents/`](./intents/)** :
    *   `contextual_lights.yaml` : Phraséologie pour les lumières.
    *   `contextual_covers.yaml` : Phraséologie pour les volets.
    *   `shortcuts.yaml` : Phraséologie pour le café et le mode dodo.
    *   `intent_scripts.yaml` : Logique de détection de pièce et routage des ordres.
*   **[`scripts/`](./scripts/)** :
    *   `k_2so_confirm_action.yaml` : Personnalité sarcastique contextuelle.
    *   `gerer_eclairage.yaml` : Intelligence centrale d'éclairage.
    *   `notification_dynamique_alexa.yaml` : Transport vocal intelligent.
*   **[`Templates/`](./Templates/)** :
    *   `presence_piece.yaml` : **Version Pro** gérant les conflits et le ciblage Echo automatique.

## 🚀 Procédure de Déploiement

### 1. Volet Vocal (Sentences)
Copiez les fichiers de phrases (`contextual_*.yaml` et `shortcuts.yaml`) vers :
- `/share/speech-to-phrase/custom_sentences/fr/`
- `/config/custom_sentences/fr/`
*Puis redémarrez l'add-on Speech-to-Phrase.*

### 2. Volet Scripts (Via l'Interface HA)
Pour chaque fichier dans le dossier `scripts/` :
1. Allez dans **Paramètres > Automatisations et scènes > Scripts**.
2. Créez un nouveau script, passez en **Mode YAML** (via les 3 points en haut à droite).
3. Copiez-collez le contenu du fichier YAML correspondant.

### 3. Volet Cœur (Via configuration.yaml)
Ajoutez ces lignes dans votre `configuration.yaml` :

```yaml
intent_script: !include intent_scripts.yaml
template: !include template.yaml
```
*Note : Assurez-vous que votre fichier `intent_scripts.yaml` est à la racine de `/config/` et ajoutez le contenu de `presence_piece.yaml` dans votre fichier `template.yaml` global.*

### 4. Test Final
Entrez dans une pièce et dites simplement : **"Allume la lumière"** ou **"Ferme le volet"**. K-2SO saura où vous êtes et agira sur le bon équipement ! 🤖🏆
