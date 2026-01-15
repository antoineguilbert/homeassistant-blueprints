# Zendure - Mode Excédent Journée

## Description

Ce blueprint active automatiquement le mode **smart/excédent solaire** sur votre batterie Zendure pendant la journée pour optimiser l'utilisation de votre production photovoltaïque.

Le mode excédent permet à la batterie de se charger uniquement avec le surplus de production solaire, évitant ainsi de consommer de l'électricité du réseau en journée.

## Fonctionnalités

### Activation automatique

- **Déclenchement horaire** : Active le mode excédent à l'heure configurée (par défaut 6h00)
- **Persistance au redémarrage** : Se réactive automatiquement au démarrage de Home Assistant si dans la plage horaire
- **Plage horaire intelligente** : Fonctionne uniquement pendant la période définie (par défaut 6h00 - 22h00)

### Notifications optionnelles

- **Alertes paramétrables** : Activez ou non les notifications selon vos préférences
- **Information batterie** : Niveau de charge inclus dans la notification

## Prérequis

- Home Assistant avec intégration Zendure configurée
- Entités Zendure :
  - Entité Zendure Manager (select) avec option "smart" disponible
  - Capteur de niveau de batterie (%)

## Installation

### Via l'interface Home Assistant

1. Accédez à **Paramètres** > **Automatisations et scènes** > **Blueprints**
2. Cliquez sur **Importer un Blueprint**
3. Collez l'URL suivante :
```
https://github.com/antoineguilbert/homeassistant-blueprints/blob/main/zendure-mode-excedent-journee/zendure_mode_excedent_journee.yaml
```
4. Cliquez sur **Aperçu** puis **Importer**

### Manuel

Copiez le fichier `zendure_mode_excedent_journee.yaml` dans le dossier `blueprints/automation/` de votre configuration Home Assistant.

## Configuration

### Paramètres obligatoires

| Paramètre | Description |
|-----------|-------------|
| **Zendure Manager** | Entité de contrôle Zendure (ex: `select.zendure_manager`) |
| **Niveau de batterie** | Capteur du niveau de charge en % (ex: `sensor.zendure_battery_level`) |

### Paramètres optionnels

| Paramètre | Valeur par défaut | Description |
|-----------|-------------------|-------------|
| **Heure de début** | 06:00:00 | Heure d'activation du mode excédent |
| **Heure de fin** | 22:00:00 | Heure de désactivation du mode excédent |
| **Activer les notifications** | false | Recevoir une notification à l'activation |
| **Service de notification** | notify.notify | Service pour les notifications |

## Fonctionnement

### Scénario type d'une journée

1. **6h00** → Activation automatique du mode smart/excédent
2. **Journée** → La batterie se charge uniquement avec le surplus solaire
3. **22h00** → Fin de la plage horaire (vous pouvez utiliser un autre blueprint pour gérer la soirée/nuit)

### Déclencheurs

L'automation se déclenche dans deux cas :
- **À l'heure de début configurée** (par défaut 6h00)
- **Au démarrage de Home Assistant** (si l'heure actuelle est dans la plage horaire)

Cela garantit que le mode excédent est toujours activé pendant la journée, même après un redémarrage.

## Exemple d'utilisation

### Configuration recommandée pour optimiser le solaire

```yaml
Heure de début: 06:00:00
Heure de fin: 22:00:00
Notifications: Activées (pour surveiller)
```

### Combinaison avec d'autres blueprints

Ce blueprint fonctionne parfaitement avec :
- **Zendure - Charge Progressive Tempo** : Pour la charge nocturne
- Un blueprint de décharge pour la soirée (peak shaving)

## Cas d'usage

- **Autoconsommation maximale** : Stockez votre surplus solaire au lieu de le réinjecter
- **Économie d'énergie** : Évitez de charger la batterie avec l'électricité du réseau en journée
- **Optimisation photovoltaïque** : Utilisez intelligemment votre production solaire

## Personnalisation

Vous pouvez adapter le blueprint selon vos besoins :
- Ajuster les horaires selon votre ensoleillement local
- Modifier les plages horaires selon les saisons
- Combiner avec des conditions météo pour plus de finesse

## Notes

- Le mode "smart" doit être disponible sur votre entité Zendure Manager
- Pensez à désactiver les autres modes de charge pendant cette plage horaire
- Compatible avec les automatisations de décharge pour optimiser la gestion batterie 24/7

## Support

Pour toute question ou suggestion d'amélioration, n'hésitez pas à ouvrir une issue sur GitHub.

## Licence

Ce blueprint est fourni tel quel, libre d'utilisation et de modification.
