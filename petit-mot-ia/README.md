# Petit mot IA du jour

[![Importer dans Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fantoineguilbert%2Fhomeassistant-blueprints%2Fblob%2Fmain%2Fpetit-mot-ia%2Fpetit_mot_ia.yaml)

## Description

Ce blueprint fait écrire par une IA un **petit mot utile** pour l'écran de la maison (tablette murale, carte du tableau de bord, notification...), quatre fois par jour :

> Aujourd'hui c'est férié, Armistice. Demain à 12 h 30, rendez-vous chez le dentiste.

> Prends un parapluie, il va pleuvoir entre 9 h et 13 h.

Le blueprint choisit lui-même les **3 infos les plus utiles du moment** dans vos données (colis, rendez-vous, jour férié, Tempo, pluie, gel, soleil, voiture). L'IA ne fait que les mettre en forme. Le soir, il parle du lendemain.

## Pourquoi le tri se fait dans le blueprint et pas dans l'IA

La première idée est de tout donner à l'IA avec une liste de règles (« par ordre de priorité... », « 180 caractères maximum »). Avec un modèle à raisonnement comme Nemotron, ça tourne mal : il réfléchit jusqu'à épuiser sa limite de jetons et ne répond rien.

Le blueprint fait donc le tri lui-même, en Jinja, et ne transmet que deux ou trois lignes déjà choisies. L'IA répond en quelques secondes, et le résultat ne dépend plus de son humeur.

## Fonctionnalités

- **Tri automatique** des infos, dans cet ordre de priorité :
  1. un colis déposé aujourd'hui ;
  2. le prochain rendez-vous dans les 36 h ;
  3. un jour férié ;
  4. un jour Tempo blanc ou rouge (« machines après 22 h ») ;
  5. la pluie entre 7 h et 21 h ;
  6. le gel ;
  7. un bon créneau de soleil pour lancer une machine ;
  8. la voiture à brancher.
- **Phrase sur la météo** quand il n'y a rien de spécial.
- **Bascule vers le lendemain** le soir (18 h par défaut).
- **Relance immédiate** quand une caméra détecte un colis, ou quand on appuie sur un bouton.
- **Toutes les sources sont facultatives**, sauf la météo : le blueprint ne parle que de ce que vous lui donnez.
- **Repli en cas de panne** : si l'IA ne répond pas (quota, coupure), le mot précédent reste affiché.
- **N'importe quelle IA** : OpenAI, Google Gemini, Anthropic, Ollama en local, Nemotron via l'API NVIDIA...

## Prérequis

### Un agent de conversation

Une intégration d'IA configurée dans Home Assistant, avec un agent de conversation.

**Conseil** : créez un agent **dédié**, sans accès au contrôle des appareils (case « Assist » décochée dans les options de l'agent).

- Il répond plus vite, parce qu'il ne reçoit pas la liste de toutes vos entités à chaque appel.
- Il n'y a aucun risque qu'il agisse sur la maison.

Exemple de prompt pour cet agent :

```
Tu écris les petits textes affichés sur l'écran d'une maison familiale. Tu écris en
français, ton détendu et naturel, comme un proche qui glisse un mot sur le frigo.
Tu n'inventes jamais rien : tu t'appuies uniquement sur les données fournies.
```

### Une aide « Texte »

Créez-la dans **Paramètres** > **Appareils et services** > **Entrées** > **Créer une entrée** > **Texte**, avec une **longueur maximale de 255**. Le mot y sera rangé.

### Une entité météo

Celle de votre intégration météo (Météo-France, Met.no...).

### Facultatif

| Source | Intégration conseillée |
|--------|------------------------|
| Agendas | Google Agenda, CalDAV, agenda local... |
| Jours fériés | **Jours fériés** (Holiday), pays France |
| Couleur Tempo | **RTE Tempo** : deux capteurs, couleur du jour et du lendemain (Bleu, Blanc, Rouge) |
| Prévision solaire | **Solcast** : capteurs « Prévisions pour aujourd'hui » et « pour demain » |
| Colis | Caméras qui signalent un colis par une entité `event` de type `package` (UniFi Protect...) |
| Voiture | Un capteur de batterie en %, et un capteur binaire « câble branché » si vous en avez un |

## Installation

### Via l'interface Home Assistant

1. Accédez à **Paramètres** > **Automatisations et scènes** > **Blueprints**
2. Cliquez sur **Importer un Blueprint**
3. Collez l'URL suivante :
```
https://github.com/antoineguilbert/homeassistant-blueprints/blob/main/petit-mot-ia/petit_mot_ia.yaml
```
4. Cliquez sur **Aperçu** puis **Importer**

### Manuel

Copiez le fichier `petit_mot_ia.yaml` dans le dossier `blueprints/automation/` de votre configuration Home Assistant.

## Configuration

### IA et rangement du texte

| Paramètre | Valeur par défaut | Description |
|-----------|-------------------|-------------|
| **Agent de conversation** | *(obligatoire)* | L'IA qui rédige |
| **Aide texte où ranger le mot** | *(obligatoire)* | L'aide « Texte » créée plus haut |
| **Consignes de style** | Tutoie, ton détendu et chaleureux, pas de « Bonjour ». | Ajoutées à chaque demande |
| **Nombre d'infos au plus** | 3 | Au-delà, le mot devient une liste |

### Quand écrire

| Paramètre | Valeur par défaut | Description |
|-----------|-------------------|-------------|
| **Matin, Midi, Fin d'après-midi, Soir** | 6 h 30, 11 h 30, 16 h 30, 19 h 30 | Les quatre rédactions de la journée |
| **Parler du lendemain à partir de** | 18 h | Après cette heure, le mot parle de demain |
| **Bouton pour relancer** | *(vide)* | Une aide « Bouton » : chaque appui fait écrire un nouveau mot |

### Données de la maison

| Paramètre | Valeur par défaut | Description |
|-----------|-------------------|-------------|
| **Météo** | *(obligatoire)* | Entité météo |
| **Agendas** | *(vide)* | Un ou plusieurs calendriers |
| **Calendrier des jours fériés** | *(vide)* | Celui de l'intégration Jours fériés |
| **Couleur Tempo du jour / de demain** | *(vide)* | Capteurs RTE Tempo |
| **Prévision solaire du jour / de demain** | *(vide)* | Capteurs Solcast |
| **Bon soleil à partir de** | 3 kWh | Production prévue au-delà de laquelle le mot conseille de lancer une machine |
| **Détection de colis** | *(vide)* | Entités `event` des caméras |
| **Batterie de la voiture** | *(vide)* | Capteur en % |
| **Voiture branchée** | *(vide)* | Capteur binaire, allumé quand le câble est branché |
| **Rappeler de brancher sous** | 30 % | Seuil de rappel |

## Fonctionnement

### Déroulé d'une rédaction

1. Le blueprint récupère les prévisions météo (heure par heure et par jour).
2. Il lit vos autres sources. Pour les agendas et les jours fériés, il se sert des attributs des calendriers (prochain événement) : pas d'appel en plus.
3. Il garde au plus 3 infos, dans l'ordre de priorité.
4. Il demande à l'IA d'en faire une ou deux phrases courtes.
5. Il range le résultat dans l'aide texte (255 caractères au plus).

### Afficher le mot

Une carte Markdown suffit :

```yaml
type: markdown
content: "{{ states('input_text.petit_mot') }}"
```

Ou dans une notification :

```yaml
action: notify.mobile_app_mon_telephone
data:
  message: "{{ states('input_text.petit_mot') }}"
```

### Une journée type

| Heure | Le blueprint dit à l'IA | L'IA écrit |
|-------|-------------------------|------------|
| 6 h 30 | Pluie prévue entre 8 h et 10 h. | Prends un parapluie ce matin, il va pleuvoir entre 8 h et 10 h. |
| 11 h 30 | Agenda : Dentiste, aujourd'hui à 17 h 30. Demain, jour Tempo rouge… | Dentiste à 17 h 30. Demain c'est Tempo rouge : les machines attendront 22 h. |
| 14 h 05 | *(colis détecté)* Un colis a été déposé devant la maison vers 14 h 05. | Un colis t'attend devant la porte depuis 14 h 05. |
| 19 h 30 | Demain, jour férié : Armistice. | Demain c'est férié, l'Armistice : grasse matinée permise. |

*(Exemples indicatifs : la formulation change à chaque fois.)*

## Personnalisation

- **Ton** : changez les « Consignes de style » (« vouvoie », « sois bref », « ajoute une touche d'humour »...).
- **Plus d'infos** : passez à 4 ou 5 infos sur un grand écran.
- **Autres moments** : décalez les heures, ou ajoutez une automatisation « Exécuter les actions » pour une rédaction à la demande.
- **Plusieurs écrans** : créez plusieurs automatisations, avec chacune son aide texte et ses consignes.

## Notes

- **Confidentialité** : à chaque rédaction, quelques lignes partent chez le fournisseur de l'IA (titre du prochain rendez-vous, heure d'un colis, météo...). Avec un modèle local (Ollama), rien ne sort de chez vous.
- **Colis** : le blueprint regarde le **dernier** événement de chaque caméra. Si la caméra détecte une personne après le colis, les rédactions suivantes ne parleront plus du colis, seulement celle déclenchée au moment de la détection.
- **Agendas** : seul le prochain événement de chaque calendrier est lu, ce qui suffit pour « le prochain rendez-vous dans les 36 h ».
- **Tempo** : les capteurs doivent valoir Bleu, Blanc ou Rouge (ou blue, white, red).
- **Longueur** : une aide texte ne garde que 255 caractères. Le mot fait une ou deux phrases courtes, donc ça passe, mais gardez 3 infos au plus.
- Testé avec Nemotron 3.5 Super (API NVIDIA, via l'intégration llama.cpp) sur Home Assistant 2026.11.

## Support

Pour toute question ou suggestion d'amélioration, n'hésitez pas à ouvrir une issue sur GitHub.

## Licence

Ce blueprint est fourni tel quel, libre d'utilisation et de modification.

---

Si ce blueprint vous est utile, vous pouvez me soutenir :

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/F1F51RMT0S)
