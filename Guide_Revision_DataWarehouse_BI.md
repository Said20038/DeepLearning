# Guide de Révision : Data Warehouse et Business Intelligence

## 📚 Chapitre 1 : Fondements des Data Warehouses

---

## 1. Système d'Information Décisionnel (SID)

### 🎯 Définition
Un **Système d'Information Décisionnel (SID)** est un ensemble de données organisées de manière spécifique pour faciliter :
- La production de tableaux de bord
- La prise de décision stratégique
- L'analyse des performances de l'entreprise

**Autres appellations :**
- Système d'Aide à la Décision (SAD)
- Business Intelligence System (BI)

### 🔄 Différence : SI Traditionnel vs SID

| Aspect | SI Traditionnel (OLTP) | SID (OLAP) |
|--------|------------------------|------------|
| **Objectif** | Gérer les opérations quotidiennes | Analyser et aider à la décision |
| **Traitement** | Temps réel, transactions rapides | Requêtes complexes, analyses |
| **Données** | Actuelles, détaillées | Historiques, agrégées |
| **Exemples** | Gestion commandes, facturation, stocks | Tableaux de bord, rapports analytiques |

**Point clé :** Le SID ne remplace pas le SI transactionnel, il le complète. C'est un écosystème parallèle qui consomme les données via l'ETL.

---

## 2. OLTP vs OLAP

### 📊 Comparaison Détaillée

| Critère | OLTP (Transactionnel) | OLAP (Analytique) |
|---------|----------------------|-------------------|
| **Finalité** | Exécuter des opérations métier (vente, réservation) | Analyser des données pour décider |
| **Modèle** | Schéma normalisé (3FN) - évite redondances | Schéma dénormalisé (étoile/flocon) - accélère requêtes |
| **Traitement** | Transactions courtes (CRUD) | Requêtes complexes (agrégats, jointures) |
| **Données** | Courantes, détaillées, mises à jour fréquentes | Historiques, agrégées, stables |
| **Volume** | Faible à moyen | Grands volumes |
| **Technologies** | SGBD relationnels (Oracle, MySQL) | Data Warehouses (Snowflake, Redshift, BigQuery) |

### 💡 Exemples Concrets

**OLTP :**
> "Une Honda Civic a été vendue par Jane Doe dans le point de vente de Londres le 1er janvier 2016"
- Requête simple, enregistrement d'une transaction

**OLAP :**
> "Trouver pour chaque région et année : les ventes totales, la moyenne mensuelle des ventes, et le nombre de clients distincts ayant acheté des Honda Civic"
- Requête complexe, analyse multidimensionnelle

---

## 3. Architecture d'un SID

### 🏗️ Composants Principaux

```
[Sources de données] → [ETL] → [Data Warehouse] → [Outils OLAP/BI] → [Utilisateurs]
```

#### 1️⃣ **Sources de données**
- Bases de données opérationnelles
- Fichiers Excel
- CRM, ERP
- Applications métiers

#### 2️⃣ **ETL (Extract-Transform-Load)**
- **Extract** : Extraction des données sources
- **Transform** : Nettoyage, harmonisation, transformation
- **Load** : Chargement dans l'entrepôt

#### 3️⃣ **Data Warehouse (DW)**
- Base de données centralisée
- Optimisée pour l'analyse
- Données historisées et intégrées

#### 4️⃣ **Outils d'analyse & visualisation**
- Tableaux de bord
- Rapports
- Outils OLAP
- Visualisation (Power BI, Tableau, Qlik)

---

## 4. Data Warehouse : Définitions et Caractéristiques

### 📖 Définitions Clés

**Définition 1 :**
Un DW est une base de données construite par copie et réorganisation de multiples sources pour servir aux applications décisionnelles.

**Définition 2 :**
Un entrepôt de données orienté sujet, intégré, non volatile et variant dans le temps, conçu pour soutenir les décisions.

**Définition 3 :**
Un DW est un entrepôt centralisé de données destiné à l'aide à la décision, stockant des données historiques provenant de sources multiples.

### ✨ Caractéristiques Essentielles

1. **Orienté Sujet** : Organisé par thème métier (ventes, clients, produits)
2. **Intégré** : Données fusionnées de différentes sources, harmonisées
3. **Non Volatile** : Données stables, pas de modifications fréquentes
4. **Historisé** : Conservation des données anciennes pour analyses temporelles
5. **Fiable** : Données nettoyées, sans doublons

---

## 5. Architectures Data Warehouse

### 🏛️ Architecture 1 : Modèle d'Inmon (Top-Down)

#### Principe
Privilégie un entrepôt centralisé unique (Enterprise Data Warehouse - EDW), hautement normalisé (3FN).

#### Structure
```
Sources → Staging Area → EDW (3FN) → Data Marts (par département) → Cubes OLAP → Utilisateurs
```

#### Couches de l'Architecture

**1. Source Layer (Couche Source)**
- Données brutes des systèmes sources
- Hétérogènes (formats, structures variés)
- Non adaptées à l'analyse

**2. Staging Area (Zone de Transit)**
- Collecte des données sources
- Nettoyage, standardisation, contrôle qualité
- Temporaire (écrasée à chaque chargement)
- Non accessible aux utilisateurs

**3. Data Warehouse Layer (Entrepôt Central)**
- Modèle en 3FN (normalisé)
- Contient toutes les données de l'entreprise
- Cohérence globale garantie

**4. Data Marts (Magasins de Données)**
- Sous-ensembles thématiques du DW
- Par département (finance, RH, marketing)
- Modèle en étoile pour faciliter l'accès
- Dérivés du DW central

**5. Cubes OLAP (Optionnel)**
- Structures pour accélérer l'analyse multidimensionnelle
- Utiles pour gros volumes et agrégations complexes

#### ✅ Avantages
- Cohérence globale des données
- Intégrité élevée
- Maintenance centralisée

#### ❌ Inconvénients
- Coûteux et complexe
- Délais de mise en œuvre longs
- Nécessite expertise pointue

---

### 🏛️ Architecture 2 : Modèle Kimball (Bottom-Up)

#### Principe
Construction progressive de Data Marts départementaux avant intégration en entrepôt global.

#### Structure
```
Sources → ETL → Data Marts (étoile/flocon) → Intégration logique → Enterprise DW
```

#### Approche
1. Commencer par des data marts départementaux (ventes, marketing)
2. Utiliser des schémas en étoile/flocon
3. Partager des dimensions conformes entre data marts
4. Intégration logique pour former un DW d'entreprise

#### ✅ Avantages
- Rapidité de déploiement (mois vs années)
- Simplicité pour utilisateurs métier
- Coûts initiaux bas
- ROI rapide

#### ❌ Inconvénients
- Redondances (dénormalisation)
- Maintenance complexe à grande échelle
- Risque d'incohérences entre data marts

---

### 📊 Comparaison Inmon vs Kimball

| Critère | Inmon (Top-Down) | Kimball (Bottom-Up) |
|---------|------------------|---------------------|
| **Approche** | Descendante | Ascendante |
| **Intégration** | Entreprise globale | Domaines métier individuels |
| **Délais** | Long (années) | Rapide (mois) |
| **Coûts initiaux** | Élevés | Bas |
| **Maintenance** | Centralisée, simple | Complexe (multiplicité) |
| **Modèle** | Normalisé (3FN) | Dénormalisé (étoile) |
| **Compétences** | Experts données + Normalisation | Généralistes + Modélisation dim. |
| **Flexibilité** | Nécessite stabilité sources | Adapté aux changements |

---

### 🏛️ Architecture 3 : Data Vault

#### Principe
Architecture hybride combinant les avantages de la 3FN et du schéma en étoile.

#### Historique
- Créé par Dan Linstedt (années 1990-2002)
- Data Vault V2.0 introduite en 2013

#### Trois Piliers
1. **Méthodologie** : Approche structurée de développement
2. **Architecture** : Organisation des couches de données
3. **Modélisation** : Hubs, Links, Satellites

#### Contexte d'Utilisation
- Gros volumes de données
- Sources multiples et évolutives
- Besoin d'historisation complète et fiable
- Environnements dynamiques et complexes
- Paradigme du lakehouse

---

## 6. Modélisation des Data Warehouses

### 🌟 Modélisation Dimensionnelle

#### Principe
Organise les données en **faits** (mesures numériques) et **dimensions** (contextes d'analyse).

---

### ⭐ Schéma en Étoile (Star Schema)

#### Structure
- **1 table de faits centrale** : objet d'étude (ventes, achats, production)
- **Tables de dimensions dénormalisées** : autour de la table de faits

#### Table de Faits

**Contenu :**
- Mesures quantitatives (ventes, quantités, montants)
- Clés étrangères vers dimensions (date_id, product_id, customer_id)

**Types de Faits :**

1. **Faits Additifs** : Peuvent être additionnés sur toutes dimensions
   - Exemples : Total ventes, Quantité vendue, Nombre de visiteurs

2. **Faits Semi-additifs** : Additionnables sur certaines dimensions seulement
   - Exemples : Solde bancaire, Stock restant, Nombre d'abonnés

3. **Faits Non-additifs** : Ne peuvent pas être additionnés (ratios, moyennes)
   - Exemples : Taux de satisfaction, Moyenne des notes, Ratio de rentabilité

#### Tables de Dimensions

**Rôle :**
- Décrire les axes d'analyse
- Permettre analyses multi-dimensionnelles
- Contenir hiérarchies naturelles (Année > Trimestre > Mois > Jour)

**Contenu :**
- Clé primaire (surrogate key)
- Attributs descriptifs riches
- Hiérarchies intégrées

**Exemples :**
- Dimension Date : jour, mois, trimestre, année
- Dimension Magasin : ville, département, pays
- Dimension Employé : prénom, nom, date de naissance

#### 📏 Concept de Grain

**Définition :** Le grain définit le niveau de détail de chaque ligne dans la table de faits.

**Exemple :** Si le grain = "une vente individuelle par jour, produit et client"
- Dimension Temps : dates au niveau du jour
- Dimension Produit : chaque produit individuellement
- Dimension Client : chaque client (pas de groupes)

**Importance :** Évite erreurs d'analyse et double comptage.

#### ✅ Règles et Bonnes Pratiques

**Structure :**
- Une seule table de faits centrale
- Dimensions dénormalisées (plates)
- Jointures par clés étrangères

**Table de Faits :**
- Uniquement faits numériques et mesurables
- Grain précis et cohérent
- Éviter valeurs nulles (utiliser valeurs par défaut)
- Distinguer types de faits (additifs, semi-additifs, non-additifs)

**Tables de Dimensions :**
- Attributs descriptifs riches
- Éviter normalisation excessive
- Valeurs par défaut pour cas inconnus (N/A, Inconnu)
- Gestion des dimensions à évolution lente (SCD)
- Dimension Date quasi-systématique

---

### ❄️ Schéma en Flocon (Snowflake Schema)

#### Principe
Variante du schéma en étoile où les dimensions sont **normalisées** (découpées en sous-tables).

#### Caractéristiques
- Dimensions réparties sur plusieurs tables liées
- Plus normalisé (moins redondant) que l'étoile
- Hiérarchies explicites

#### ⚠️ Recommandations
- **Généralement déconseillé** : performances diminuées par jointures multiples
- **Normalisation partielle préconisée** : uniquement si répétitions très nombreuses sur beaucoup de colonnes

---

### 🔄 Slowly Changing Dimensions (SCD)

#### Problématique
Certaines informations changent avec le temps (adresse client, nom, statut). Comment gérer ces évolutions ?

#### Type 0 : Conserver la Valeur d'Origine

**Principe :** La valeur ne change jamais
- Faits toujours associés à valeur initiale
- Utilisé pour données "originales" (score crédit initial, ID permanent)
- Très courant pour dimension Temps

#### Type 1 : Écrasement

**Principe :** Remplacer ancienne valeur par nouvelle
- Seule la valeur récente est conservée
- **Historique effacé**
- Facile à implémenter
- ⚠️ Nécessite recalcul des agrégats

**Exemple :**
```
Avant : Client 123 | Paris
Après : Client 123 | Lyon
```

#### Type 2 : Ajout d'une Nouvelle Ligne ⭐ (Le plus utilisé)

**Principe :** Conserver historique complet en ajoutant une nouvelle ligne

**Colonnes supplémentaires :**
- Date de début de validité
- Date de fin de validité
- Indicateur de ligne courante (actif/inactif)

**Exemple :**
```
Avant :
| ID | Client_ID | Ville  | Date_Début | Date_Fin   | Actif |
|----|-----------|--------|------------|------------|-------|
| 1  | 123       | Paris  | 2023-01-01 | 9999-12-31 | Oui   |

Après déménagement (15 mars 2024) :
| ID | Client_ID | Ville  | Date_Début | Date_Fin   | Actif |
|----|-----------|--------|------------|------------|-------|
| 1  | 123       | Paris  | 2023-01-01 | 2024-03-14 | Non   |
| 2  | 123       | Lyon   | 2024-03-15 | 9999-12-31 | Oui   |
```

**Avantages :**
- Historique complet préservé
- Analyses temporelles précises

#### Type 3 : Ajout d'un Nouvel Attribut

**Principe :** Ajouter colonne pour stocker ancienne valeur
- Valeur principale mise à jour
- Ancienne version gardée dans autre champ
- **Rarement utilisé** : ne suit qu'un seul changement

**Exemple :**
```
| Client_ID | Ville_Actuelle | Ville_Précédente |
|-----------|----------------|------------------|
| 123       | Lyon           | Paris            |
```

#### Type 4 : Table d'Historique Séparée

**Principe :** Table principale non modifiée + table d'historique séparée
- Moins de surcharge sur table principale
- Complexité accrue pour jointures

**Structure :**
```
Table Principale : Employe (données actuelles)
Table Historique : Employe_History (tous les changements)
```

#### 📚 Autres Types (5, 6, 7)
Voir ressources Kimball Group pour techniques avancées.

---

### 🔢 Modélisation en 3ème Forme Normale (3FN)

#### Contexte
- Standard pour bases transactionnelles (OLTP)
- Base propre, fiable, bien structurée
- **Trop complexe pour utilisateurs métiers en DW**
- Préférer modèles dénormalisés pour analyses

#### Rappel des Formes Normales

**1FN (Première Forme Normale) :**
- Toute cellule contient une seule valeur atomique
- Pas de listes, pas de tableaux dans colonnes

**2FN (Deuxième Forme Normale) :**
- Être en 1FN
- Aucune dépendance partielle à clé primaire composite
- Toutes colonnes dépendent entièrement de la clé primaire

**3FN (Troisième Forme Normale) :**
- Être en 2FN
- Aucune dépendance transitive
- Colonnes dépendent directement de la clé primaire (pas d'autres colonnes non-clés)

---

### 🏗️ Modélisation Data Vault

#### Définition
Modèle de conception pour créer un DW à des fins d'analytique à l'échelle de l'entreprise.

#### Architecture Hybride
Combine avantages de :
- **3FN** : Intégrité, non-redondance
- **Schéma en étoile** : Performance pour l'analyse

#### Composants : Hubs, Links, Satellites

---

#### 🔵 Hubs

**Définition :** Représentent les concepts métier fondamentaux (clients, produits, magasins)

**Contenu :**
- Clé métier (identifiant du système source)
- Clé technique (générée par l'entrepôt)
- Champs d'audit (horodatage, source)

**Caractéristiques :**
- Une seule ligne par clé métier
- **NE CONTIENT PAS** : attributs descriptifs, historique, relations
- Centralise uniquement les identifiants uniques

**Exemple : Hub_Client**
```
| Hub_Client_ID | Client_ID_Metier | Date_Chargement | Source |
|---------------|------------------|-----------------|--------|
| 1             | C12345           | 2024-01-15      | CRM    |
```

---

#### 🔗 Links

**Définition :** Représentent les relations entre Hubs

**Rôle :**
- Modéliser associations métier (client passe commande)
- Capturer relations observées dans systèmes sources

**Contenu :**
- Clés des Hubs liés
- Clé technique du Link
- Champs d'audit
- **Aucun attribut descriptif**

**Caractéristiques :**
- Permettent relations n:m
- Facilitent évolution du modèle
- Peuvent avoir leurs propres Satellites pour historiser la relation

**Exemple : Link_Commande_Client**
```
| Link_ID | Hub_Client_ID | Hub_Commande_ID | Date_Chargement |
|---------|---------------|-----------------|-----------------|
| 1       | 1             | 101             | 2024-01-20      |
```

**Cas d'usage avancé :**
Si relation évolue (ex: statut abonnement change), créer Satellite rattaché au Link.

---

#### 🛰️ Satellites

**Définition :** Stockent les attributs descriptifs et l'historique des Hubs ou Links

**Rôle :**
- Informations contextuelles et historiques
- Attributs décrivant l'entité (nom, adresse, statut)
- Traçabilité (date chargement, source)
- Historique complet des changements

**Caractéristiques :**
- Un Hub/Link peut avoir plusieurs Satellites
- Chaque Satellite dédié à un contexte ou source spécifique
- Similaire au SCD Type 2

**Exemple : Satellite_Client_Details**
```
| Sat_ID | Hub_Client_ID | Nom    | Email           | Adresse | Date_Début | Date_Fin   |
|--------|---------------|--------|-----------------|---------|------------|------------|
| 1      | 1             | Dupont | dupont@mail.com | Paris   | 2024-01-15 | 2024-03-14 |
| 2      | 1             | Dupont | dupont@mail.com | Lyon    | 2024-03-15 | 9999-12-31 |
```

---

#### 🎯 Quand Utiliser Data Vault ?

**Contexte idéal :**
- Gros volumes de données
- Sources multiples et évolutives
- Besoin d'historisation complète et fiable
- Environnements dynamiques et complexes
- Paradigme du lakehouse

**Avantages :**
- Flexibilité maximale
- Traçabilité complète
- Évolutivité sans casser le modèle
- Intégration facilitée de nouvelles sources

---

## 📝 Points Clés à Retenir

### Concepts Fondamentaux
1. **SID ≠ SI Transactionnel** : Écosystèmes parallèles et complémentaires
2. **OLTP vs OLAP** : Transaction vs Analyse
3. **ETL** : Processus crucial de transformation des données

### Architectures
1. **Inmon (Top-Down)** : EDW centralisé → Data Marts (cohérence, coûteux)
2. **Kimball (Bottom-Up)** : Data Marts → Intégration (rapide, redondances)
3. **Data Vault** : Hybride (flexibilité, complexité)

### Modélisations
1. **Étoile** : 1 table faits + dimensions dénormalisées (performance)
2. **Flocon** : Dimensions normalisées (déconseillé généralement)
3. **3FN** : Pour OLTP, trop complexe pour DW
4. **Data Vault** : Hubs + Links + Satellites (évolutivité)

### SCD (Slowly Changing Dimensions)
- **Type 0** : Jamais de changement
- **Type 1** : Écrasement (pas d'historique)
- **Type 2** : Nouvelle ligne (historique complet) ⭐
- **Type 3** : Nouvelle colonne (1 seul changement)
- **Type 4** : Table d'historique séparée

---

## 🎓 Conseils de Révision

### Pour l'examen
1. **Maîtriser les différences** : OLTP/OLAP, Inmon/Kimball
2. **Savoir modéliser** : Schéma en étoile avec faits et dimensions
3. **Comprendre les SCD** : Surtout Type 1 et Type 2
4. **Connaître les composants** : ETL, DW, Data Marts, Cubes OLAP
5. **Data Vault** : Hubs, Links, Satellites et leurs rôles

### Exercices Pratiques
- Dessiner un schéma en étoile pour un cas métier
- Identifier le type de SCD approprié selon le contexte
- Comparer architectures selon critères (coût, délai, maintenance)
- Modéliser en Data Vault (identifier Hubs, Links, Satellites)

---

**Bonne révision ! 📚✨**

*Guide créé à partir du cours du Professeur Abdelhadi Bouain*
