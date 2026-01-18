# 🤖 Phase 2.2 : Intégration K-2SO (Personnalité Vocale)

Cette phase transforme votre domotique en un véritable assistant sarcastique en intégrant **K-2SO** (le droïde de Rogue One) dans chaque interaction vocale.

## 🎯 Objectif

Ajouter une **personnalité vocale** à toutes vos commandes domotiques, tout en conservant la logique intelligente de la Phase 2.1 (scènes dynamiques, détection contextuelle).

## 🆕 Nouveautés par rapport à la Phase 2.1

| Aspect | Phase 2.1 | Phase 2.2 (K-2SO) |
|--------|-----------|-------------------|
| **Exécution** | Script intelligent (`gerer_eclairage`) | ✅ Identique |
| **Réponse vocale** | Notification standard | 🤖 **K-2SO personnalisé** |
| **Créativité** | Message fixe | 🎲 **Généré par IA (Ollama)** |
| **Ton** | Neutre | 😏 **Sarcastique et geek** |

## 📂 Structure

```
phase_2.2/
├── intents/
│   ├── contextual_lights.yaml      # Phrases de déclenchement (inchangé)
│   ├── contextual_covers.yaml      # Phrases volets (inchangé)
│   ├── shortcuts.yaml              # Phrases raccourcis (inchangé)
│   └── intent_scripts.yaml         # ⭐ NOUVEAU : Logique avec K-2SO
├── Templates/
│   └── satellite_actif_memorise.yaml  # Détection de pièce (inchangé)
└── README.md
```

## 🔧 Pré-requis

### 1. Script K-2SO
Vous devez avoir le script `k_2so_generateur_de_message` dans votre `/config/scripts.yaml`.

> [!TIP]
> Le script K-2SO est disponible dans le repository : [k2so-home-assistant-ia](https://github.com/votre-repo/k2so-home-assistant-ia)

### 2. Script de Notification Alexa
Le script `notification_alexa` doit être configuré pour gérer :
- La détection automatique de l'Echo actif (via `sensor.presence_piece`)
- La gestion du volume
- La pause/reprise de Spotify

- Configuration : `keep_alive: -1` pour que le modèle reste en VRAM.
- Optimisation GPU :
  ```bash
  OLLAMA_MAX_LOADED_MODELS=1
  OLLAMA_NUM_PARALLEL=1
  ```

## 🚀 Installation

1. **Copier les fichiers de phrases** :
   ```bash
   cp phase_2.2/intents/*.yaml /config/custom_sentences/fr/
   # (Sauf intent_scripts.yaml)
   ```

2. **Intégrer la logique** :
   Ajoutez le contenu de `intent_scripts.yaml` dans votre `/config/intent_scripts.yaml`

3. **Template de détection** :
   Ajoutez `Templates/satellite_actif_memorise.yaml` dans `/config/template.yaml`

4. **Recharger** :
   - Outils de développement > YAML > Recharger les Intents
   - Outils de développement > YAML > Recharger les Templates

## 🎭 Exemples de Réponses K-2SO

### Lumières
- *"Lumos !"* → **"Lumière du salon activée. Encore une facture qui grimpe."**
- *"Bravo six, passage au noir"* → **"Extinction confirmée. Économie d'énergie niveau djédaï."**

### Volets
- *"Protocole Bunker"* → **"Tous les volets fermés. Mode forteresse rebelle activé."**
- *"Ouvrez l'iris"* → **"Volets ouverts. Toute la maison est maintenant visible de l'extérieur. Brillant."**

### Raccourcis
- *"Overclocking humain"* → **"Café en préparation. Mission caféine activée. Votre dépendance est notée."**
- *"Entrée en stase"* → **"Mode dodo activé. Que les rêves soient avec vous."**

## ⚙️ Personnalisation

Chaque intent utilise le champ `consigne` pour guider K-2SO :

```yaml
- action: script.k_2so_generateur_de_message
  data:
    mission: "lumiere_allumee"
    details: "{{ piece }}"
    consigne: "IMPÉRATIF : MAXIMUM 10 MOTS. Sarcastique."
  response_variable: generated_message
```

Vous pouvez modifier les `consigne` pour ajuster le ton et la longueur. Le mot **IMPÉRATIF** garantit que K-2SO respectera la limite de mots :
- Plus bref : *"ORDRE : 5 MOTS MAX. Très sarcastique."*
- Plus geek : *"IMPÉRATIF : 10 MOTS MAX. Référence à l'Etoile de la Mort."*
- Plus sec : *"IMPÉRATIF : SOIS TRÈS BREF. Juste les faits."*

## 🧪 Tests

1. **Test basique** :
   - Dites *"Lumos"* dans une pièce
   - Vérifiez que la lumière s'allume ET que K-2SO commente

2. **Test global** :
   - Dites *"Blackout"*
   - Toutes les lumières doivent s'éteindre avec un commentaire sarcastique

3. **Test fallback** :
   - Coupez Ollama temporairement
   - Les actions doivent quand même s'exécuter (avec message de secours)

## 🔄 Retour à la Phase 2.1

Si vous préférez revenir aux notifications standards :
1. Restaurez l'ancien `intent_scripts.yaml` de la Phase 2.1
2. Rechargez les Intents

## 📊 Performance

- **Latence constatée** : ~2.5s (avec NVIDIA GTX 1050 Ti 4GB).
- **Fiabilité** : Actions exécutées AVANT l'IA (pas de blocage)
- **Créativité** : Réponses variées à chaque fois

---

**Prêt à donner une personnalité à votre maison ?** 🤖✨

**[Retour à la Phase 2.1](../phase_2.1/)** | **[Voir le script K-2SO](https://github.com/votre-repo/k2so-home-assistant-ia)**
