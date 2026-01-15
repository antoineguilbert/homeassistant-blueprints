# Zendure - Charge Progressive Tempo

## Description

Ce blueprint permet de charger automatiquement votre batterie Zendure de manière progressive en fonction :
- Du **niveau de charge actuel** de la batterie
- De la **couleur Tempo prévue** par RTE (Blanc ou Rouge)

La puissance de charge s'adapte automatiquement selon trois paliers pour optimiser la charge et éviter une surconsommation inutile.

## Fonctionnalités

### Charge progressive adaptative

Le blueprint ajuste automatiquement la puissance de charge selon le niveau de batterie :

| Niveau de batterie | Puissance par défaut | Mode |
|-------------------|---------------------|------|
| < 30% | 1700W | Charge rapide |
| 30% - 60% | 1200W | Charge normale |
| > 60% | 600W | Charge d'appoint |

### Déclenchement intelligent

- **Déclenchement horaire** : La charge démarre à l'heure configurée (par défaut 22h00)
- **Condition Tempo** : Ne se déclenche que les jours Blanc ou Rouge pour profiter des tarifs avantageux
- **Notifications** : Vous êtes informé du mode de charge activé avec le niveau de batterie actuel

## Prérequis

- Home Assistant avec intégration Zendure configurée
- Capteur RTE Tempo configuré (fournissant les couleurs : Blanc, Bleu, Rouge)
- Entités Zendure :
  - Capteur de niveau de batterie (%)
  - Entité Zendure Manager (select)
  - Entité de limite d'entrée (number)
  - Entité de mode AC (select)

## Installation

### Via l'interface Home Assistant

1. Accédez à **Paramètres** > **Automatisations et scènes** > **Blueprints**
2. Cliquez sur **Importer un Blueprint**
3. Collez l'URL suivante :
```
https://github.com/[VOTRE-USERNAME]/homeassistant-blueprints/blob/main/zendure-charge-progressive-tempo/zendure_charge_progressive_tempo.yaml
```
4. Cliquez sur **Aperçu** puis **Importer**

### Manuel

Copiez le fichier `zendure_charge_progressive_tempo.yaml` dans le dossier `blueprints/automation/` de votre configuration Home Assistant.

## Configuration

### Paramètres obligatoires

| Paramètre | Description |
|-----------|-------------|
| **Capteur Tempo** | Entité capteur RTE Tempo (ex: `sensor.rte_tempo_couleur_demain`) |
| **Niveau de batterie** | Capteur du niveau de charge en % (ex: `sensor.zendure_battery_level`) |
| **Zendure Manager** | Entité de contrôle Zendure (ex: `select.zendure_manager`) |
| **Limite d'entrée** | Entité de puissance de charge (ex: `number.zendure_input_limit`) |
| **Mode AC** | Entité du mode AC (ex: `select.zendure_ac_mode`) |

### Paramètres optionnels

| Paramètre | Valeur par défaut | Description |
|-----------|-------------------|-------------|
| **Heure de déclenchement** | 22:00:00 | Heure de début de charge |
| **Seuil batterie faible** | 30% | En dessous : charge rapide |
| **Seuil batterie moyenne** | 60% | En dessous : charge normale |
| **Puissance charge rapide** | 1700W | Puissance si batterie < 30% |
| **Puissance charge normale** | 1200W | Puissance si batterie 30-60% |
| **Puissance charge d'appoint** | 600W | Puissance si batterie > 60% |
| **Service de notification** | notify.notify | Service pour les notifications |

## Exemple d'utilisation

Votre batterie Zendure sera chargée chaque nuit à 22h, uniquement les jours Blanc ou Rouge :

1. **Si batterie à 25%** → Charge rapide à 1700W avec notification
2. **Si batterie à 45%** → Charge normale à 1200W avec notification
3. **Si batterie à 75%** → Charge d'appoint à 600W avec notification

## Personnalisation

Vous pouvez ajuster les seuils et puissances selon vos besoins :
- Modifier les horaires de charge
- Adapter les paliers de puissance à votre installation
- Changer les seuils de batterie selon votre usage
- Personnaliser le service de notification

## Support

Pour toute question ou suggestion d'amélioration, n'hésitez pas à ouvrir une issue sur GitHub.

## Licence

Ce blueprint est fourni tel quel, libre d'utilisation et de modification.
