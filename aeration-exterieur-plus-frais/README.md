# Notification Aération - Extérieur plus frais qu'à l'intérieur

[![Importer dans Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fantoineguilbert%2Fhomeassistant-blueprints%2Fblob%2Fmain%2Faeration-exterieur-plus-frais%2Faeration_exterieur_plus_frais.yaml)

## Description

Ce blueprint vous envoie une notification dès que la température extérieure passe **sous** celle de la pièce la plus chaude de votre logement. C'est le bon moment pour ouvrir les fenêtres et rafraîchir naturellement, sans climatisation.

Typiquement utile en fin de journée l'été : dès que l'air extérieur redevient plus frais que l'intérieur, vous êtes prévenu.

## Fonctionnalités

- **Comparaison multi-pièces** : surveillez autant de capteurs intérieurs que vous voulez, la notification part dès que l'extérieur passe sous la **plus chaude** d'entre elles
- **Marge configurable** : évite les notifications quand les deux températures sont quasi égales
- **Durée de stabilisation** : la condition doit rester vraie X minutes, ce qui évite les fausses alertes sur un pic de mesure isolé
- **Message dynamique** : la notification indique la température extérieure et la liste des pièces concernées
- **Conditions libres** : ajoutez vos propres conditions (présence, plage horaire, interrupteur dédié...) sans modifier le blueprint
- **Notification au choix** : compatible avec l'app mobile, les notifications persistantes, Telegram, TTS, etc.

## Prérequis

### Entités nécessaires

- **Un capteur de température extérieure** : capteur physique (Zigbee, Z-Wave...) ou capteur d'une intégration météo (Météo-France, OpenWeatherMap...)
- **Un ou plusieurs capteurs de température intérieurs** : thermomètres des pièces à surveiller

> Les capteurs doivent avoir la `device_class: temperature` pour apparaître dans les sélecteurs.

### Service de notification

Un moyen de notification configuré dans Home Assistant, par exemple :

- **[Home Assistant Companion](https://companion.home-assistant.io/)** pour les notifications mobiles
- Les notifications persistantes (aucune configuration nécessaire)
- Telegram, Discord, Slack, un haut-parleur pour du TTS...

## Installation

### Via l'interface Home Assistant

1. Accédez à **Paramètres** > **Automatisations et scènes** > **Blueprints**
2. Cliquez sur **Importer un Blueprint**
3. Collez l'URL suivante :
```
https://github.com/antoineguilbert/homeassistant-blueprints/blob/main/aeration-exterieur-plus-frais/aeration_exterieur_plus_frais.yaml
```
4. Cliquez sur **Aperçu** puis **Importer**

### Manuel

Copiez le fichier `aeration_exterieur_plus_frais.yaml` dans le dossier `blueprints/automation/` de votre configuration Home Assistant.

## Configuration

### Paramètres obligatoires

| Paramètre | Description |
|-----------|-------------|
| **Capteur extérieur** | Capteur de température mesurant l'extérieur (ex: `sensor.temperature_exterieure`) |
| **Capteurs à surveiller** | Un ou plusieurs capteurs de température intérieurs |
| **Notification à envoyer** | L'action de notification à déclencher |

### Paramètres optionnels

| Paramètre | Valeur par défaut | Description |
|-----------|-------------------|-------------|
| **Marge de déclenchement** | 1 °C | Écart minimum entre l'extérieur et la pièce la plus chaude |
| **Durée de stabilisation** | 5 minutes | Durée pendant laquelle la condition doit rester vraie |
| **Conditions supplémentaires** | *(vide)* | Vos propres conditions : présence, plage horaire, interrupteur... |

## Fonctionnement

### Logique de déclenchement

L'automatisation se déclenche quand la condition suivante devient vraie :

```
température extérieure < (température de la pièce la plus chaude - marge)
```

Puis elle attend la **durée de stabilisation** avant de notifier. Si la condition redevient fausse pendant ce délai, aucune notification n'est envoyée.

Comme il s'agit d'un déclencheur de type *template*, la notification ne part **qu'une seule fois par franchissement** : elle ne se répète pas tant que la situation ne s'est pas inversée puis rétablie.

### Scénario type d'une journée d'été

1. **14h00** → 32 °C dehors, 27 °C dans le salon : rien ne se passe
2. **20h30** → 27 °C dehors, 28 °C dans le salon : l'écart est encore sous la marge, rien ne se passe
3. **21h15** → 26 °C dehors, 28 °C dans le salon : la condition devient vraie
4. **21h20** → la condition est toujours vraie après 5 minutes → **notification envoyée**
5. **Nuit** → plus aucune notification, même si l'écart continue de se creuser

### Message généré

Le blueprint construit automatiquement deux variables utilisables dans votre notification :

| Variable | Exemple de contenu |
|----------|--------------------|
| `title` | `🌬️ Aération possible` |
| `message` | `Il fait 26.0°C dehors, plus frais qu'à l'intérieur (Salon, Chambre). Bon moment pour aérer !` |

Pour les utiliser, configurez votre action de notification ainsi :

```yaml
action: notify.mobile_app_mon_telephone
data:
  title: "{{ title }}"
  message: "{{ message }}"
```

Rien ne vous empêche d'écrire votre propre texte à la place si vous préférez.

## Exemples d'utilisation

### Configuration simple

```yaml
Capteur extérieur: sensor.temperature_exterieure
Capteurs à surveiller:
  - sensor.temperature_salon
  - sensor.temperature_chambre
Marge de déclenchement: 1 °C
Durée de stabilisation: 5 minutes
```

### Uniquement quand quelqu'un est à la maison

Dans **Conditions supplémentaires**, ajoutez une condition d'état :

```yaml
- condition: state
  entity_id: group.famille
  state: "home"
```

### Uniquement le soir, et pas la nuit

```yaml
- condition: time
  after: "18:00:00"
  before: "23:00:00"
```

### Seulement s'il fait vraiment chaud à l'intérieur

```yaml
- condition: numeric_state
  entity_id: sensor.temperature_salon
  above: 25
```

### Avec un interrupteur pour désactiver les notifications

```yaml
- condition: state
  entity_id: input_boolean.notifications_aeration
  state: "on"
```

## Cas d'usage

- **Rafraîchissement naturel** : ouvrez les fenêtres au bon moment plutôt que de faire tourner la clim
- **Confort nocturne** : profitez de la fraîcheur du soir pour faire baisser la température des chambres
- **Économie d'énergie** : la ventilation naturelle est gratuite

## Personnalisation

- **Marge plus élevée (2-3 °C)** : moins de notifications, mais uniquement quand l'écart est vraiment intéressant
- **Stabilisation plus longue (10-15 min)** : utile si votre capteur extérieur est bruyant ou exposé au soleil
- **Plusieurs automatisations** : créez-en une par étage ou par zone, avec des marges différentes

## Notes

- Placez votre capteur extérieur à l'ombre : un capteur au soleil renvoie une température faussée et retarde la notification
- Les capteurs indisponibles (`unavailable`, `unknown`) sont ignorés dans le calcul et ne déclenchent pas de fausse alerte
- Le blueprint compare uniquement les températures : il ne tient pas compte de l'humidité ni de la qualité de l'air
- Pensez à un blueprint complémentaire pour vous rappeler de **refermer** les fenêtres au matin

## Support

Pour toute question ou suggestion d'amélioration, n'hésitez pas à ouvrir une issue sur GitHub.

## Licence

Ce blueprint est fourni tel quel, libre d'utilisation et de modification.

---

Si ce blueprint vous est utile, vous pouvez me soutenir :

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/F1F51RMT0S)
