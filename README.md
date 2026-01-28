# Delta.Dom — Energy Tariff Engine (Community Edition)

Ce dépôt fournit un **moteur tarifaire** Home Assistant (local) :  
**contrat → plages horaires → prix courant €/kWh**.

⚠️ Ce module **ne mesure pas** la consommation et **ne lit pas Linky**.  
Il **applique des tarifs** à partir de helpers (`input_select`, `input_number`) et expose un capteur de **prix courant**.

---

## 1) Structure du dépôt

À la racine :
- `LICENSE`
- `CHANGELOG.md`
- `README.md`
- `deltadom/deltadom_energy_tariff_ce/`
- `example/`

Dans `example/` :
- `blueprint/automation/*.yaml`
- `lovelace/*.yaml`
  - `10_helpers.yaml`
  - `20_sensors.yaml`
  - `30_scripts.yaml`
  - `40_automations.yaml`

---

## 2) Installation

### Étape 1 — Copier le package CE dans Home Assistant

1. Ouvrir le dossier du dépôt :
   - `moteur_gestion_tariffaire_DeltaDomOS/deltadom /deltadom_energy_tariff_ce/`

2. Copier **le dossier entier** `deltadom/deltadom_energy_tariff_ce`

3. Coller dans Home Assistant, au chemin :
   - `/config//config/deltadom/deltadom_energy_tariff_ce

Résultat attendu côté Home Assistant :
/config/deltadom/deltadom_energy_tariff_ce/
10_helpers.yaml
20_sensors.yaml
30_scripts.yaml
40_automations.yaml


⚠️ Ne pas renommer le dossier.  
⚠️ Ne pas renommer les fichiers.

---








### Étape 2 — Ajouter 1 ligne dans `configuration.yaml`

Dans `configuration.yaml`, sous `homeassistant: packages:`, ajouter la ligne suivante :

```yaml
homeassistant:
  packages:
    deltadom_energy_tariff_ce: !include_dir_merge_named deltadom/deltadom_energy_tariff_ce ’’’


Le chemin doit être deltadom/deltadom_energy_tariff_ce

 
Étape 3 — Redémarrer Home Assistant
Faire un redémarrage complet de Home Assistant.

Étape 4 — Vérifier que le module est chargé
Dans Home Assistant : Outils de développement → États
Vérifier que ces entités existent :

input_select.ce_contract_type

input_select.ce_tempo_day_type

sensor.ce_price_current_eur_kwh

binary_sensor.ce_is_hc

binary_sensor.ce_is_hsc

Si elles existent : le moteur est opérationnel.


3) Utilisation

3.1 Choisir le contrat
Modifier :
 
input_select.ce_contract_type

3.2 Si contrat Tempo
Modifier :
 
input_select.ce_tempo_day_type (Bleu / Blanc / Rouge)




3.3 Lire le prix courant
Le prix courant est dans :

 
sensor.ce_price_current_eur_kwh


4) Blueprint (optionnel) — synchroniser un Tempo externe
Le blueprint sert uniquement à copier une valeur Bleu/Blanc/Rouge depuis un capteur externe (Linky/API/MQTT…) vers :

input_select.ce_tempo_day_type

Étape 1 — Copier le blueprint  dans Home Assistant
Depuis le dépôt :

example/blueprint/automation/deltadom_tempo_manual_price.yaml

Copier le fichier YAML

Coller dans Home Assistant :

/config/blueprints/automation/

Exemple :

/config/blueprints/automation/deltadom_tempo_manual_price.yaml
 
Étape 2 — Créer l’automatisation depuis le blueprint
Dans Home Assistant :

Paramètres → Automatisations & scènes → Blueprints

Sélectionner le blueprint Delta.Dom CE
 
Renseigner :

Tempo source entity : l’entité externe qui donne Bleu/Blanc/Rouge
 
Optional attribute name : vide (sauf si la valeur est dans un attribut)

Target input_select (CE) : input_select.ce_tempo_day_type
 
Normalize : ON

⚠️ Si vous n’avez pas de capteur Tempo externe : n’utilisez pas le blueprint.Vasy demerde toi parceque je vais serrer encore la 
