# 🧠 Phase 2.1 : Conscience & Centralisation de l'Éclairage

Cette phase marque le passage d'une domotique de "commande" à une domotique de "comportement". Elle conserve toute l'architecture modulaire de la **Phase 2**, mais introduit un **cerveau central** pour vos lumières.

## 🎯 La Philosophie de Centralisation (DRY)

Le changement majeur est l'utilisation du script **`gerer_eclairage.yaml`**. 

**Pourquoi ce script ?**
Au lieu de répéter la même logique (choix de scène, vérification du soleil, sécurité) dans chaque bouton, chaque automatisation de mouvement et chaque commande vocale, **tout est centralisé ici**.
- **Cohérence Totale** : Que vous disiez *"Lumos"*, que vous appuyiez sur un bouton physique ou que vous entriez dans la pièce, le résultat sera identique et prévisible.
- **Maintenance Simplifiée** : Si vous voulez changer l'heure de passage au mode "Veilleuse", vous ne le faites qu'à un seul endroit.

## 🚀 Fonctionnalités Intelligentes
- **Scènes Dynamiques** : Choix automatique (Veilleuse / Atténué / Stimulation) basé sur le cycle Jour/Nuit et l'élévation du soleil.
- **Priorité Vocale** : Une commande vocale est prioritaire sur les capteurs de mouvement.
- **Boucliers de Sécurité** : Ne s'enclenche pas si la maison est vide.
- **Gestion des Sommeils** : Blocage intelligent de l'allumage dans la chambre si quelqu'un est détecté dans le lit.

## 📂 Architecture de la Phase
*   **[`scripts/gerer_eclairage.yaml`](./scripts/gerer_eclairage.yaml)** : Le moteur central (le Cerveau).
*   **[`helpers/infra_intelligence.yaml`](./helpers/infra_intelligence.yaml)** : L'infrastructure temporelle (L'Horloge Interne).
*   **[`intents/intent_scripts.yaml`](./intents/intent_scripts.yaml)** : Passerelle vocale vers le script.

---

## 🧭 Comprendre l'Infrastructure (`infra_intelligence.yaml`)

Le script a besoin de savoir "depuis quand" vous avez changé d'état pour gérer les transitions douces (ex: le mode réveil). Dans ce projet, la notion de Temps est **liée à l'utilisateur** et non seulement au soleil :
- **Mode JOUR** : L'utilisateur est **éveillé** et la maison est active.
- **Mode NUIT** : L'utilisateur et la maison sont en **mode "Dodo"**.

**Les outils de l'horloge interne :**
- **`input_datetime`** : Enregistre le passage précis de "Dodo" à "Éveillé" (et inversement).
- **`input_text`** : L'état actuel de la maison ("jour" ou "nuit").
- **`sensor`** : Rend cette donnée temporelle exploitable par le cerveau du script.
- **`automation`** : Rafraîchit le calcul toutes les 5 minutes pour une précision optimale.

## 🛠️ Comment Adapter le Système ?

Pour que la magie opère, vous devez faire correspondre vos entités réelles avec les variables du script :

1.  **Entités Globales** : Dans le script, vérifiez `sensor.etat_canabang_et_device_tracker`. Remplacez-le par votre sensor de présence global.
2.  **Sécurités Spécifiques** : 
    - **Chambre** : Remplacez `binary_sensor.esp_bed_occupation_master_bed_occupied` par votre capteur de lit (ou supprimez la condition).
    - **SdB** : Remplacez `sensor.lux_sdb` par votre capteur de luminosité pour éviter d'allumer si le soleil suffit.
3.  **Naming des scènes** : Le script cherche des scènes nommées `scene.hue_<piece>_1_veilleuse`. Assurez-vous que vos scènes suivent ce pattern ou adaptez le script (Lignes 57-61).

---

## 🚀 Procédure de Déploiement Rapide

1.  **Infrastructure** : Déployez les éléments de [`helpers/infra_intelligence.yaml`](./helpers/infra_intelligence.yaml).
2.  **Script Central** : Importez [`scripts/gerer_eclairage.yaml`](./scripts/gerer_eclairage.yaml) dans vos scripts HA.
3.  **Intents** : Utilisez le [`intents/intent_scripts.yaml`](./intents/intent_scripts.yaml) de cette phase.
4.  **Adaptation** : Modifiez les Entity IDs dans le script pour qu'ils correspondent à vos capteurs (Lit, Présence, Lux).
5.  **Recharge** : Rechargez les scripts, les templates et les intents dans Home Assistant.

---

**Le socle technique est maintenant prêt pour une gestion "Pro" de vos lumières. Prochaine étape : K-2SO !** 🤖💎
