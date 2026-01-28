# Delta.Dom — Energy Tariff Engine (Community Edition)

Ce dépôt fournit un moteur tarifaire Home Assistant (local) :
contrat → plages horaires → prix courant €/kWh.

ATTENTION :
- Ce module ne mesure PAS la consommation.
- Ce module ne lit PAS Linky.
- Il applique des tarifs à partir de helpers (input_select, input_number) et expose un capteur de prix courant.

---

## 1) Structure du dépôt

À la racine :
- LICENSE
- CHANGELOG.md
- README.md
- deltadom/deltadom_energy_tariff_ce/
- example/

Dans example/ :
- blueprint/automation/*.yaml
- lovelace/*.yaml

Dans deltadom/deltadom_energy_tariff_ce/ :
- 10_helpers.yaml
- 20_sensors.yaml
- 30_scripts.yaml
- 40_automations.yaml

---

## 2) Installation

### Étape 1 — Copier le package CE dans Home Assistant

1) Ouvrir le dossier du dépôt :
   moteur_gestion_tariffaire_DeltaDomOS/deltadom/deltadom_energy_tariff_ce/

2) Copier le dossier entier :
   deltadom_energy_tariff_ce

3) Coller dans Home Assistant au chemin EXACT :
   /config/deltadom/deltadom_energy_tariff_ce/

Résultat attendu côté Home Assistant :

/config/deltadom/deltadom_energy_tariff_ce/
- 10_helpers.yaml
- 20_sensors.yaml
- 30_scripts.yaml
- 40_automations.yaml

Image :
images/1.png

IMPORTANT :
- Ne pas renommer le dossier
- Ne pas renommer les fichiers

---

### Étape 2 — Ajouter 1 ligne dans configuration.yaml

Dans configuration.yaml, sous homeassistant → packages, ajouter :

homeassistant:
  packages:
    deltadom_energy_tariff_ce: !include_dir_merge_named deltadom/deltadom_energy_tariff_ce

Le chemin doit être EXACTEMENT :
deltadom/deltadom_energy_tariff_ce

---

### Étape 3 — Redémarrer Home Assistant

Faire un redémarrage COMPLET de Home Assistant.

---

### Étape 4 — Vérifier que le module est chargé

Dans Home Assistant :
Outils de développement → États

Vérifier que ces entités existent :

- input_select.ce_contract_type
- input_select.ce_tempo_day_type
- sensor.ce_price_current_eur_kwh
- binary_sensor.ce_is_hc
- binary_sensor.ce_is_hsc

Si elles existent : le moteur est opérationnel.

---

## 3) Utilisation

### 3.1 Choisir le contrat

Modifier l’entité :
input_select.ce_contract_type

Image :
images/2.png

---

### 3.2 Contrat Tempo (manuel)

Modifier l’entité :
input_select.ce_tempo_day_type

Valeurs possibles :
Bleu / Blanc / Rouge

Image :
images/3.png

IMPORTANT :
- Le changement de jour Tempo n’est PAS automatique sans source externe.

---

### 3.3 Lire le prix courant

Le prix courant est disponible dans :
sensor.ce_price_current_eur_kwh

Image :
images/4.png

---

## 4) Blueprint (OPTIONNEL) — Synchroniser un Tempo externe

Ce blueprint sert UNIQUEMENT à copier une valeur Bleu / Blanc / Rouge depuis un capteur externe
(Linky / API / MQTT / autre) vers :

input_select.ce_tempo_day_type

### Étape 1 — Copier le blueprint

Depuis le dépôt :
example/blueprint/automation/deltadom_tempo_manual_price.yaml

Copier ce fichier vers Home Assistant :
/config/blueprints/automation/deltadom_tempo_manual_price.yaml

Image :
images/5.png

---

### Étape 2 — Créer l’automatisation depuis le blueprint

Dans Home Assistant :
Paramètres → Automatisations & scènes → Blueprints

Sélectionner :
Delta.Dom CE – Tempo day type → input_select

Image :
images/6.png

Renseigner :
- Tempo source entity : entité externe donnant Bleu / Blanc / Rouge
- Optional attribute name : laisser vide (sauf si la valeur est dans un attribut)
- Target input_select (CE) : input_select.ce_tempo_day_type
- Normalize : ON

Images :
images/7.png
images/8.png

ATTENTION :
- Si vous n’avez PAS de capteur Tempo externe, N’UTILISEZ PAS le blueprint.
