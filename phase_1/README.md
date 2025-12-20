# 🧪 Phase 1 : Validation Technique (Salon)

Cette phase est un "bac à sable" conçu pour valider votre installation vocale (micro, STT, Assist) avec une seule pièce : le **Salon**. 

Elle est totalement **autonome** et regroupe tous les fichiers nécessaires pour fonctionner de manière isolée.

## 📂 Structure du Dossier

*   **[`intents/`](./intents/)** :
    *   `lumiere_salon.yaml` : Les phrases d'activation (ex: *Lumos*, *Banane*).
    *   `intent_scripts.yaml` : La logique qui reçoit les ordres.
*   **[`scripts/`](./scripts/)** :
    *   `k_2so_confirm_action.yaml` : Personnalité sarcastique.
    *   `gerer_eclairage.yaml` : Pilotage des lumières.
    *   `notification_dynamique_alexa.yaml` : Diffusion vocale Alexa.
*   **[`Templates/`](./Templates/)** :
    *   `presence_piece_basic.yaml` : Capteur de présence simplifié pour le salon.

## 🚀 Procédure de Déploiement

### 1. Volet Vocal (Sentences)
Copiez le contenu du dossier `intents/` (hors `intent_scripts.yaml`) vers :
- `/share/speech-to-phrase/custom_sentences/fr/`
- `/config/custom_sentences/fr/`
*Puis redémarrez l'add-on Speech-to-Phrase.*

### 2. Volet Scripts (Via l'Interface HA)
Les scripts ne sont pas gérés par fichier. Pour chaque fichier dans le dossier `scripts/` :
1. Allez dans **Paramètres > Automatisations et scènes > Scripts**.
2. Créez un nouveau script, passez en **Mode YAML** (via les 3 points en haut à droite).
3. Copiez-collez le contenu du fichier YAML correspondant.

### 3. Volet Cœur (Via configuration.yaml)
Ajoutez ces lignes dans votre `configuration.yaml` pour lier les fichiers d'intents et de templates :

```yaml
intent_script: !include intent_scripts.yaml
template: !include template.yaml
```
*Note : Assurez-vous que vos fichiers `intent_scripts.yaml` et `template.yaml` sont bien placés à la racine de votre dossier `/config/`.* et ajouter le contenu de presence_piece_basic.yaml dans votre template.yaml

### 4. Test Final
Dites simplement : **"Banane"** ou **"Allume le salon"**. Si K-2SO vous répond avec sarcasme et allume la lumière, votre base technique est validée ! 🍌💡
