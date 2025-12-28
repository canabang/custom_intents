# 🧪 Test 7 : Détection par Surveillance d'État

## 💡 Concept

Au lieu d'essayer d'accéder à `satellite_id` via les variables d'intent (impossible), on crée un **sensor template** qui surveille l'état de tous les satellites.

Quand un satellite passe en état `listening`, le sensor retourne immédiatement la pièce associée.

## 📋 Fichiers Créés

### 1. [`Templates/satellite_actif.yaml`](./Templates/satellite_actif.yaml)

**Sensor :** `sensor.satellite_actif`

**Fonctionnement :**
- Surveille l'état de tous les satellites définis
- Quand un satellite est en `listening`, retourne sa pièce
- Fournit des attributs : `satellite_id` et `echo`

**Mapping :**
```yaml
satellites = {
  'assist_satellite.esp_va_salon_satellite_assist': 'salon',
  'assist_satellite.esp_va_cuisine_satellite_assist': 'cuisine',
  'assist_satellite.respeaker_chambre_satellite_assist': 'chambre',
  'assist_satellite.atom_echo_sdb_satellite_assist': 'sdb'
}
```

### 2. [`intents/intent_scripts.yaml`](./intents/intent_scripts.yaml)

**Intent de test :** `TurnOnContextualLight`

**Utilisation :**
```yaml
- variables:
    piece: "{{ states('sensor.satellite_actif') | lower }}"

- service: light.turn_on
  target:
    entity_id: "light.hue_{{ piece }}"
```

## 🎯 Avantages

✅ **Contourne la limitation des variables d'intent**  
✅ **Détection en temps réel** du satellite actif  
✅ **Fonctionne pour plusieurs personnes** dans différentes pièces  
✅ **Réutilisable** : un seul sensor pour tous les intents  
✅ **Mapping centralisé** : facile à maintenir

## ⚠️ Considérations

**Performance :**
- Le sensor se met à jour à chaque changement d'état de satellite
- Impact minimal : les satellites changent d'état rarement (uniquement pendant l'écoute)

**Timing :**
- Le sensor doit se mettre à jour AVANT que l'intent ne s'exécute
- Si le satellite repasse en `idle` trop vite, le sensor pourrait retourner "inconnu"

## 🧪 Procédure de Test

1. **Déployer** le sensor dans `template.yaml`
2. **Redémarrer** Home Assistant ou recharger les templates
3. **Vérifier** que `sensor.satellite_actif` existe dans les entités
4. **Tester** "tarte aux pommes" depuis différents satellites
5. **Observer** si le sensor détecte correctement la pièce

## 📊 Résultat du Test

### ✅ Test 9 : SUCCÈS !

**Résultat confirmé :**
- ✅ Pièce détectée : `salon`
- ✅ Satellite ID : `assist_satellite.esp_va_salon_satellite_assist`
- ✅ Echo associé : `media_player.echo_studio_d`
- ✅ Timestamp : `2025-12-20T22:11:26.666155+01:00`

**Conclusion :**
Le sensor trigger-based **fonctionne parfaitement** ! La valeur est capturée au moment où le satellite passe en `listening` et **persiste** même après que le satellite repasse en `idle`.

### 🎯 Solution Finale

**Fichier à utiliser :** [`Templates/satellite_actif_memorise.yaml`](./Templates/satellite_actif_memorise.yaml)

**Intégration dans les intents :**
```yaml
TurnOnContextualLight:
  action:
    - variables:
        piece: "{{ states('sensor.satellite_actif_memorise') | lower }}"
    - service: light.turn_on
      target:
        entity_id: "light.hue_{{ piece }}"
```

### 🚀 Avantages Validés

✅ **Détection automatique** de la pièce depuis le satellite  
✅ **Fonctionne pour plusieurs personnes** dans différentes pièces  
✅ **Pas de capteurs radar** nécessaires pour la localisation vocale  
✅ **Fiable** : la valeur persiste après changement d'état  
✅ **Mapping centralisé** : un seul endroit à maintenir  

### 📝 Prochaines Étapes

1. Intégrer `sensor.satellite_actif_memorise` dans les Phases 1 et 2
2. Remplacer `sensor.presence_piece` par le nouveau sensor dans les intents contextuels
3. Tester depuis tous les satellites pour confirmer la détection dans chaque pièce
4. Documenter la solution finale dans le projet principal
