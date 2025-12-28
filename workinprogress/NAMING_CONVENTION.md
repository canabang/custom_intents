# 📐 Convention de Nommage : La Clé de l'Automatisation

## 🎯 Pourquoi c'est Crucial

Dans ce projet, **la convention de nommage n'est pas une simple recommandation, c'est le pilier central** qui permet à tout le système de fonctionner automatiquement. Sans elle, chaque pièce nécessiterait du code spécifique.

## 🏗️ La Convention Utilisée

### Pattern Général : `<type>_<piece>`

Tous les équipements suivent ce pattern strict où `<piece>` est **toujours en minuscules** et **identique partout**.

### Exemples Concrets

#### Lumières Hue
```
light.hue_salon
light.hue_cuisine
light.hue_chambre
light.hue_sdb
```

#### Volets
```
cover.volsalon
cover.volcuisine
cover.volchambre
cover.volsdb
```

#### Satellites Assist
```
assist_satellite.esp_va_salon_satellite_assist
assist_satellite.esp_va_cuisine_satellite_assist
assist_satellite.respeaker_chambre_satellite_assist
assist_satellite.atom_echo_sdb_satellite_assist
```

#### Enceintes Echo
```
media_player.echo_studio_d          → Salon
media_player.echo_show_cuisine      → Cuisine
media_player.echo_show_chambre      → Chambre
media_player.echo_sdb               → SdB
```

## 🔑 Les Noms de Pièces Standardisés

| Pièce | Nom Standardisé | Utilisé Dans |
|-------|----------------|--------------|
| Salon | `salon` | Lumières, volets, satellites |
| Cuisine | `cuisine` | Lumières, volets, satellites |
| Chambre | `chambre` | Lumières, volets, satellites |
| Salle de Bain | `sdb` | Lumières, volets, satellites |

## 💡 Comment ça Fonctionne

### 1. Détection de la Pièce

Le sensor `sensor.satellite_actif` retourne : `"salon"`

### 2. Construction Automatique des Entity IDs

```jinja
{% set piece = states('sensor.satellite_actif') %}

{# Lumière #}
entity_id: "light.hue_{{ piece }}"  → light.hue_salon

{# Volet #}
entity_id: "cover.vol{{ piece }}"   → cover.volsalon

{# Echo #}
{% set echo_map = {
  'salon': 'media_player.echo_studio_d',
  ...
} %}
echo: {{ echo_map.get(piece) }}     → media_player.echo_studio_d
```

### 3. Un Seul Intent pour Toutes les Pièces

Au lieu de créer :
- `TurnOnSalonLight`
- `TurnOnCuisineLight`
- `TurnOnChambreLight`
- `TurnOnSdbLight`

On a **UN SEUL** intent :
```yaml
TurnOnContextualLight:
  action:
    - variables:
        piece: "{{ states('sensor.satellite_actif') }}"
    - service: light.turn_on
      target:
        entity_id: "light.hue_{{ piece }}"
```

## ⚠️ Importance de la Cohérence

### ✅ Bon Exemple (Cohérent)
```yaml
Satellite : assist_satellite.esp_va_salon_satellite_assist
Lumière  : light.hue_salon
Volet    : cover.volsalon
Echo     : (mapping manuel vers echo_studio_d)
```
→ Le système fonctionne automatiquement

### ❌ Mauvais Exemple (Incohérent)
```yaml
Satellite : assist_satellite.living_room_satellite
Lumière  : light.hue_salon
Volet    : cover.volsalon
```
→ Le mapping satellite → pièce échoue, le système ne fonctionne pas

## 🛠️ Adapter à Votre Installation

Si vous voulez utiliser ce système, vous **devez** :

1. **Renommer vos équipements** pour suivre le pattern `<type>_<piece>`
2. **Choisir des noms de pièces courts** et cohérents (ex: `salon`, `cuisine`, `chambre`, `sdb`)
3. **Utiliser EXACTEMENT les mêmes noms** partout (satellites, lumières, volets, etc.)
4. **Mettre à jour les mappings** dans :
   - `Templates/satellite_actif.yaml` (mapping satellite → pièce)
   - `Templates/presence_piece.yaml` (mapping pièce → echo)

## 🎯 Bénéfices

✅ **Code générique** : Un intent fonctionne pour toutes les pièces  
✅ **Maintenance facile** : Ajouter une pièce = ajouter une ligne dans le mapping  
✅ **Évolutivité** : Facile d'ajouter de nouveaux types d'équipements  
✅ **Lisibilité** : Le code est clair et prévisible  

## 📝 Checklist de Vérification

Avant de déployer, vérifiez que :
- [ ] Tous vos satellites ont un nom cohérent avec la pièce
- [ ] Toutes vos lumières suivent `light.hue_<piece>`
- [ ] Tous vos volets suivent `cover.vol<piece>`
- [ ] Le mapping dans `satellite_actif.yaml` est correct
- [ ] Le mapping dans `presence_piece.yaml` est correct
- [ ] Les noms de pièces sont **identiques** partout (casse comprise)

---

**Sans cette convention de nommage, le projet K-2SO ne peut pas fonctionner de manière contextuelle !** 🎯
