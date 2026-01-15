# Home Assistant - Blueprints

Collection de blueprints pour optimiser la gestion de votre batterie Zendure et d'autres automatisations Home Assistant.

## Blueprints disponibles

### 1. Zendure - Charge Progressive Tempo

Charge automatique et progressive de votre batterie Zendure en fonction du niveau de charge et de la couleur Tempo RTE.

**Caractéristiques :**
- Charge adaptative sur 3 paliers (rapide/normale/appoint)
- Déclenchement uniquement les jours Blanc ou Rouge
- Notifications du mode de charge actif
- Paramètres de puissance personnalisables

[📖 Documentation](./zendure-charge-progressive-tempo/README.md) | [📥 Blueprint](./zendure-charge-progressive-tempo/zendure_charge_progressive_tempo.yaml)

### 2. Zendure - Mode Excédent Journée

Active le mode smart/excédent solaire en journée pour optimiser l'autoconsommation de votre production photovoltaïque.

**Caractéristiques :**
- Activation automatique en journée
- Plage horaire configurable
- Persistance au redémarrage de Home Assistant
- Notifications optionnelles

[📖 Documentation](./zendure-mode-excedent-journee/README.md) | [📥 Blueprint](./zendure-mode-excedent-journee/zendure_mode_excedent_journee.yaml)

---

## Installation

### Via l'interface Home Assistant

1. Accédez à **Paramètres** > **Automatisations et scènes** > **Blueprints**
2. Cliquez sur **Importer un Blueprint**
3. Collez l'URL du blueprint souhaité (voir ci-dessous)
4. Cliquez sur **Aperçu** puis **Importer**

### URLs d'import

**Charge Progressive Tempo :**
```
https://github.com/[VOTRE-USERNAME]/homeassistant-blueprints/blob/main/zendure-charge-progressive-tempo/zendure_charge_progressive_tempo.yaml
```

**Mode Excédent Journée :**
```
https://github.com/[VOTRE-USERNAME]/homeassistant-blueprints/blob/main/zendure-mode-excedent-journee/zendure_mode_excedent_journee.yaml
```

## Combinaison recommandée

Pour une gestion optimale 24/7 de votre batterie Zendure :

- **Journée (6h-22h)** : Mode Excédent Journée → Charge avec surplus solaire
- **Nuit (22h)** : Charge Progressive Tempo → Charge sur heures creuses Tempo

## Support et contributions

Pour toute question, suggestion ou pour signaler un bug :
- Ouvrez une issue sur GitHub
- Consultez la documentation de chaque blueprint

## Source

Plus d'informations et articles sur : https://www.antoineguilbert.fr/category/domotique/home-assistant/

## Licence

Ces blueprints sont fournis tels quels, libres d'utilisation et de modification.
