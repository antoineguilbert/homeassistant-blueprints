# Home Assistant - Blueprints

Collection de blueprints pour Home Assistant : gestion de batterie solaire, confort thermique, IA et automatisations du quotidien.

## Blueprints disponibles

### 🔋 Batterie & solaire

#### Zendure - Charge Progressive Tempo

Charge automatique et progressive de votre batterie Zendure en fonction du niveau de charge et de la couleur Tempo RTE (jours Blanc/Rouge uniquement).

[![Importer dans Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fantoineguilbert%2Fhomeassistant-blueprints%2Fblob%2Fmain%2Fzendure-charge-progressive-tempo%2Fzendure_charge_progressive_tempo.yaml)

[📖 Documentation complète](./zendure-charge-progressive-tempo/README.md)

---

#### Zendure - Programmation Horaire de Mode

Active automatiquement un mode de fonctionnement (Smart, Charge, Décharge, Manuel, Arrêt) sur une plage horaire définie. Idéal pour l'excédent solaire en journée ou la décharge en heures pleines.

[![Importer dans Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fantoineguilbert%2Fhomeassistant-blueprints%2Fblob%2Fmain%2Fzendure-mode-excedent-journee%2Fzendure_mode_excedent_journee.yaml)

[📖 Documentation complète](./zendure-mode-excedent-journee/README.md)

---

### 🌡️ Confort & température

#### Notification Aération - Extérieur plus frais qu'à l'intérieur

Vous prévient dès que la température extérieure passe sous celle de la pièce la plus chaude : le bon moment pour ouvrir les fenêtres et rafraîchir naturellement.

[![Importer dans Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fantoineguilbert%2Fhomeassistant-blueprints%2Fblob%2Fmain%2Faeration-exterieur-plus-frais%2Faeration_exterieur_plus_frais.yaml)

[📖 Documentation complète](./aeration-exterieur-plus-frais/README.md)

---

### 🤖 Intelligence artificielle

#### Résumé IA du jour

Fait écrire par une IA (OpenAI, Gemini, Ollama, Nemotron...) un court résumé utile pour l'écran de la maison : colis déposé, prochain rendez-vous, jour férié, Tempo, pluie, gel... Le blueprint choisit les infos, l'IA les met en forme.

[![Importer dans Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fantoineguilbert%2Fhomeassistant-blueprints%2Fblob%2Fmain%2Fresume-ia%2Fresume_ia.yaml)

[📖 Documentation complète](./resume-ia/README.md)

---

## Installation

Chaque blueprint s'importe en un clic via le bouton **Importer dans Home Assistant** ci-dessus, ou manuellement :

1. Accédez à **Paramètres** > **Automatisations et scènes** > **Blueprints**
2. Cliquez sur **Importer un Blueprint**
3. Collez l'URL GitHub du fichier `.yaml` du blueprint
4. Cliquez sur **Aperçu** puis **Importer**

Consultez la documentation de chaque blueprint pour le détail des prérequis et des paramètres.

## Source

Plus d'informations sur : [antoineguilbert.fr](https://www.antoineguilbert.fr/category/domotique/home-assistant/)

## Licence

Ces blueprints sont fournis tels quels, libres d'utilisation et de modification.

---

Si ces blueprints vous sont utiles, vous pouvez me soutenir :

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/F1F51RMT0S)
