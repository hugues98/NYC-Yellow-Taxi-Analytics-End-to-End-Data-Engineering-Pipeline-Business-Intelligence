!<img width="602" height="396" alt="image" src="https://github.com/user-attachments/assets/c42fab0a-330a-4a0a-a8b6-ade2a021dbd7" />
# 🚕 NYC Yellow Taxi Analytics
### End-to-End Data Engineering Pipeline & Business Intelligence avec Databricks




[![Databricks](https://img.shields.io/badge/Databricks-Platform-red?logo=databricks)](https://www.databricks.com/)
[![PySpark](https://img.shields.io/badge/PySpark-Data%20Processing-orange?logo=apachespark)](https://spark.apache.org/docs/latest/api/python/)
[![Delta Lake](https://img.shields.io/badge/Delta%20Lake-Storage-blue)](https://delta.io/)
[![SQL](https://img.shields.io/badge/SQL-Analytics-lightgrey)](https://www.databricks.com/product/databricks-sql)
[![Looker Studio](https://img.shields.io/badge/Looker%20Studio-Business%20Intelligence-4285F4?logo=looker)](https://lookerstudio.google.com/)


Projet de **Data Engineering & Analytics** visant à construire un pipeline de traitement de données de bout en bout à partir des données des taxis jaunes de New York (*NYC Yellow Taxi*).

L'objectif est de transformer les données brutes en indicateurs analytiques à travers une architecture **Medallion (Bronze → Silver → Gold)** et de les exploiter dans un dashboard avec Looker Studio.

## Objectifs du projet

1. **Bronze :** ingestion des données sources CSV/Parquet.
2. **Silver :** nettoyage, normalisation et enrichissement avec PySpark.
3. **Gold :** agrégation des données pour l'analyse.
4. **Orchestration :** automatisation des traitements avec Databricks Workflows.
5. **Visualisation :** création d'un dashboard analytique.

## Architecture & Workflow

Le pipeline repose sur trois couches :

- **Bronze :** stockage des données brutes.
- **Silver :** nettoyage, traitement des valeurs aberrantes et création des colonnes `weekday_name` et `route`.
- **Gold :** préparation des indicateurs analytiques.

Les traitements sont orchestrés avec Databricks Workflows.

!<img width="716" height="126" alt="Capture d&#39;écran 2026-11-08 203704" src="https://github.com/user-attachments/assets/8c94aaa3-f7c9-42a8-8508-06cd59b22bda" />


## Extraction SQL

Les données de la table `processed_trips_with_zones` sont agrégées avec Databricks SQL pour alimenter le dashboard.

```sql
SELECT
    pickup_date,
    pickup_borough,
    dropoff_borough,
    CONCAT(pickup_borough, '-', dropoff_borough) AS route,
    HOUR(tpep_pickup_datetime) AS pickup_hour,
    ROUND(AVG(trip_distance), 2) AS avg_distance,
    COUNT(*) AS number_trips,
    ROUND(SUM(total_amount), 2) AS total_amount
FROM processed_trips_with_zones
GROUP BY
    pickup_date,
    pickup_borough,
    dropoff_borough,
    HOUR(tpep_pickup_datetime)
ORDER BY
    pickup_date,
    pickup_hour;
```
!<img width="653" height="324" alt="Capture d&#39;écran 2026-11-08 205048" src="https://github.com/user-attachments/assets/7a4be566-7eab-4ac0-9c79-a5168365b0b0" />




## Connexion Databricks SQL → Looker Studio

Pour connecter les données à Looker Studio, j'utilise le connecteur Databricks disponible dans Looker Studio.

La connexion se déroule en plusieurs étapes :

1. **Préparation dans Databricks :** démarrage d'un SQL Warehouse pour rendre les tables analytiques accessibles.
2. **Récupération des paramètres :** depuis les détails de connexion du SQL Warehouse, récupération du Server Hostname et création d'un Personal Access Token.
3. **Configuration dans Looker Studio :** ajout du connecteur Databricks, autorisation de l'accès et saisie des identifiants de connexion.
4. **Sélection des données :** choix du SQL Warehouse et définition de la requête SQL permettant de récupérer les données préparées.
5. **Création du dashboard :** utilisation des données connectées pour construire les graphiques et les indicateurs analytiques.

Le flux de connexion est le suivant :

`Databricks (Gold) → SQL Warehouse → Connecteur Databricks → Looker Studio → Dashboard`

Cette intégration permet d'exploiter les données transformées avec PySpark et agrégées avec SQL dans un environnement de Business Intelligence.

!<img width="454" height="378" alt="Capture d&#39;écran 2026-11-09 012811" src="https://github.com/user-attachments/assets/ce6186b9-d9ab-4753-8abe-99df3e8fa309" />


## Dashboard analytique

Le dashboard permet de suivre :

- Le nombre de courses par jour et par heure.
- Les distances moyennes par itinéraire.
- Les revenus selon les lieux de départ et d'arrivée.
- Les périodes de forte demande.

!<img width="944" height="433" alt="Capture d&#39;écran 2026-11-08 170225" src="https://github.com/user-attachments/assets/2347b9f9-c774-47b8-8f99-f1bf5a53fff6" />


## Stack technique

| Technologie | Utilisation |
|---|---|
| Databricks | Plateforme Data Engineering |
| Apache Spark / PySpark | Traitement et transformation |
| Delta Lake | Stockage des données |
| Python | Développement |
| SQL / Databricks SQL | Extraction et agrégation |
| Databricks Workflows | Orchestration des traitements |
| Looker Studio | Business Intelligence et visualisation |

## Compétences mises en pratique

- Ingestion et transformation de données.
- Architecture Medallion et Data Lakehouse.
- Traitement avec PySpark et SQL.
- Orchestration de pipelines ETL.
- Analyse et visualisation de données.

## Ressources

- **Code source :** [GitHub Repository](https://glorious-palm-tree-xpj65gwvqrgcrj7.github.dev/)
- **Dashboard :** [Looker Studio](https://datastudio.google.com/reporting/1c419126-e671-4025-9076-7bc327eba984)

*Projet personnel réalisé à des fins d'apprentissage pratique à partir d'un tutoriel technique.*
