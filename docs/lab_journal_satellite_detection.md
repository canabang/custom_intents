# 🧪 Work In Progress : Détection du Satellite Vocal

## 🎯 Objectif

Déterminer quelle variable ou méthode permet d'identifier **quel assistant vocal (satellite) est en train d'écouter**, et donc de déduire automatiquement la pièce où se trouve l'utilisateur.

## 🔬 Hypothèses à Tester

Lors de l'exécution d'un intent, plusieurs variables contextuelles pourraient être disponibles :

1.  **`device_id`** : ID du device qui a capté la commande vocale
2.  **`satellite_id`** : ID spécifique du satellite vocal (si disponible)
3.  **`area_id(device_id)`** : Zone associée au device
4.  **`area`** : Variable directe de zone (si elle existe)

## 📋 Méthodologie de Test

### Fichiers Modifiés

*   **`intent_scripts.yaml`** : Contient un intent `DiagnosticSatellite` qui teste toutes les variables possibles
*   **`lumiere_salon.yaml`** : Phrases de déclenchement incluant "diagnostic satellite"

### Procédure de Test

1.  **Déploiement** :
    *   Copier `lumiere_salon.yaml` dans `/config/custom_sentences/fr/` et `/share/speech-to-phrase/custom_sentences/fr/`
    *   Copier le contenu de `intent_scripts.yaml` dans votre configuration HA
    *   Redémarrer l'add-on Speech-to-Phrase

2.  **Exécution** :
    *   Depuis **différents satellites** (BOX-3, ReSpeaker, Atom Echo), dire : **"diagnostic satellite"**
    *   Noter ce que K-2SO répond pour chaque satellite

3.  **Analyse** :
    *   Identifier quelle(s) variable(s) change(nt) selon le satellite
    *   Vérifier si on peut mapper satellite → pièce de manière fiable

## 📊 Résultats de Test - CONCLUSION

### 🧪 Historique des Tests

#### Test 1 : Affichage Direct de `device_id`
**Méthode :** `{{ device_id }}`  
**Résultat :** ❌ Affiche `<function DeviceExtension.device_id at 0x7fb7f46700e0>`  
**Conclusion :** `device_id` est une référence de fonction, pas une valeur

#### Test 2 : Appel de `device_id()` comme fonction
**Méthode :** `{{ device_id() }}`  
**Résultat :** ❌ Erreur "Une erreur est intervenue pendant le traitement"  
**Conclusion :** `device_id()` n'est pas callable dans le contexte des intents

#### Test 3 : Utilisation de `area_id(device_id)`
**Méthode :** `{{ area_id(device_id) }}`  
**Résultat :** ❌ Retourne `None`  
**Conclusion :** `device_id` n'est pas utilisable comme paramètre

#### Test 4 : Capture dans variables puis utilisation
**Méthode :**
```yaml
- variables:
    dev_id: "{{ device_id }}"
- service: persistent_notification.create
  data:
    message: "{{ dev_id }}"
```
**Résultat :** ❌ Affiche toujours `<function ...>`  
**Conclusion :** Le contexte ne change pas entre variables et notification

#### Test 5 : Test direct de `satellite_id` dans variables
**Méthode :**
```yaml
- variables:
    sat_id: "{{ satellite_id }}"
```
**Résultat :** ❌ Retourne `non_defini`  
**Conclusion :** `satellite_id` n'est pas défini dans le contexte variables

#### Test 7 : Utilisation directe de `satellite_id` dans service
**Méthode :**
```yaml
- service: persistent_notification.create
  data:
    message: "{{ satellite_id }}"
```
**Résultat :** ❌ Affiche `<function DeviceExtension.device_id at ...>` pour device_id, "NON" pour satellite_id  
**Conclusion :** `satellite_id` n'est pas accessible même en utilisation directe

#### Test 8 : Sensor template surveillant l'état des satellites
**Méthode :**
```yaml
- sensor:
    - name: "Satellite Actif"
      state: >
        {% for satellite_id, piece in satellites.items() %}
          {% if is_state(satellite_id, 'listening') %}
            {{ piece }}
          {% endif %}
        {% endfor %}
```
**Résultat :** ⚠️ Sensor fonctionne MAIS timing trop rapide  
**Problème :** Le satellite repasse en `idle` avant que l'intent ne s'exécute → sensor retourne "inconnu"  
**Conclusion :** Bonne approche mais nécessite une mémoire/rétention de la valeur

#### Test 9 : Sensor trigger-based avec mémoire ✅ RÉUSSI
**Méthode :**
```yaml
- trigger:
    - platform: state
      entity_id: assist_satellite.esp_va_salon_satellite_assist
      to: "listening"
  sensor:
    - name: "Satellite Actif Mémorisé"
      state: "{{ satellites.get(trigger.entity_id) }}"
```
**Résultat :** ✅ **FONCTIONNE PARFAITEMENT !**  
**Pièce détectée :** `salon`  
**Satellite ID :** `assist_satellite.esp_va_salon_satellite_assist`  
**Echo associé :** `media_player.echo_studio_d`  
**Conclusion :** **SOLUTION TROUVÉE !** Le sensor trigger-based capture et retient la pièce au moment du déclenchement. La valeur persiste même après que le satellite repasse en idle.

### ✅ SOLUTION FINALE VALIDÉE

Après 9 tests approfondis, nous avons trouvé la solution fonctionnelle :

**Sensor trigger-based avec mémoire** ([`Templates/satellite_actif_memorise.yaml`](./Templates/satellite_actif_memorise.yaml))

**Validation complète :**
- ✅ Testé depuis **tous les satellites** (Salon, Cuisine, Chambre, SdB)
- ✅ Détection correcte de la pièce dans chaque cas
- ✅ Mapping satellite → pièce → echo fonctionnel
- ✅ Valeur retenue même après changement d'état du satellite

**Avantages :**
- ✅ Détection automatique de la pièce depuis le satellite qui écoute
- ✅ Fonctionne pour plusieurs personnes dans différentes pièces simultanément
- ✅ Pas de dépendance aux capteurs radar pour la localisation vocale
- ✅ Valeur retenue même après changement d'état du satellite
- ✅ Mapping centralisé facile à maintenir
- ✅ Convention de nommage cohérente : `assist_satellite.esp_va_<piece>_satellite_assist`

**Utilisation dans les intents :**
```yaml
- variables:
    piece: "{{ states('sensor.satellite_actif_memorise') | lower }}"
- service: light.turn_on
  target:
    entity_id: "light.hue_{{ piece }}"
```

### 🎯 Impact sur le Projet K-2SO

Cette découverte révolutionne l'architecture du projet :

**Avant (avec `sensor.presence_piece`) :**
- Dépendance aux capteurs radar LD2410C/LD2450
- Logique de priorité complexe en cas de présence multiple
- Problème : Si 2 personnes dans 2 pièces, seule la pièce prioritaire répond

**Après (avec `sensor.satellite_actif_memorise`) :**
- Détection directe depuis le satellite qui écoute
- Chaque personne contrôle SA pièce, peu importe où sont les autres
- Plus simple, plus fiable, plus intuitif

**Exemple concret :**
```
Situation : Vous au Salon, autre personne à la SdB

Avant :
- Personne SdB dit "allume la lumière"
- sensor.presence_piece = "salon" (priorité)
- Lumière du SALON s'allume ❌

Après :
- Personne SdB dit "allume la lumière"
- sensor.satellite_actif_memorise = "sdb" (satellite qui écoute)
- Lumière de la SDB s'allume ✅
```

**Prochaines étapes :**
1. Intégrer le sensor dans Phase 1 et Phase 2
2. Option : Garder `sensor.presence_piece` pour les automations non-vocales
3. Option : Combiner les deux approches (vocal = satellite, auto = présence)

### ❌ Limitations Découvertes (Variables d'Intent)

Après 6 tests approfondis, voici ce que nous avons confirmé :

**Variables dans les logs du pipeline :**
- ✅ `satellite_id` : Présent (ex: `assist_satellite.esp_va_salon_satellite_assist`)
- ✅ `device_id` : Présent (ex: `2f36eee09bec5e31f5a646d6d2591738`)

**Variables accessibles dans les intents Jinja :**
- ❌ `satellite_id` : **NON accessible** (retourne `non_defini` ou n'existe pas)
- ❌ `device_id` : **NON accessible** (référence de fonction non-callable)
- ❌ `area_id(device_id)` : **NON fonctionnel** (device_id n'est pas utilisable)

### 🎯 Conclusion

Les variables de contexte du pipeline (`satellite_id`, `device_id`) **existent uniquement dans les logs** mais ne sont **pas exposées au contexte Jinja des intent_scripts**.

### 💡 Solutions Possibles

**Option 1 : Continuer avec `sensor.presence_piece`** (Actuel)
- ✅ Fonctionne parfaitement
- ✅ Basé sur les capteurs radar LD2410C/LD2450
- ✅ Déjà implémenté et testé

**Option 2 : Attendre une évolution de Home Assistant**
- Demander aux développeurs d'exposer `satellite_id` dans le contexte des intents
- Feature request sur GitHub

**Option 3 : Utiliser des automations déclenchées par satellite**
- Créer des automations qui écoutent les événements de chaque satellite
- Plus complexe à maintenir

### 🚀 Recommandation

**Garder l'approche actuelle avec `sensor.presence_piece`** qui combine :
- Capteurs radar pour la détection physique
- Logique de priorité et d'exclusion
- Mapping automatique vers les enceintes Echo

Cette approche est **robuste, testée et fonctionnelle** ! 🎯

## 🚧 Prochaines Étapes

**En cours de test** - Résultats à documenter après expérimentation.
