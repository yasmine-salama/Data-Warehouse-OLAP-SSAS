# Data Warehouse – Analyse et Personnalisation de Cubes OLAP avec SSAS

![Data Warehouse](https://img.shields.io/badge/Domaine-Data%20Warehouse-blue)
![SQL Server](https://img.shields.io/badge/SQL%20Server-SSAS-red)
![OLAP](https://img.shields.io/badge/Technologie-OLAP-orange)

## Présentation du projet

Ce mini-projet a été réalisé dans le cadre du module **Data Warehouse**, durant la deuxième année du DUT Ingénierie des Données à l'École Supérieure de Technologie d'El Kelâa des Sraghna (ESTK), Université Cadi Ayyad.

Il porte sur l'**analyse et la personnalisation de cubes OLAP (Online Analytical Processing)** à l'aide de Microsoft SQL Server Analysis Services (SSAS).

En s'appuyant sur la base de données **Adventure Works DW2022**, le projet couvre la construction et l'exploitation d'un environnement analytique multidimensionnel, la création de vues analytiques personnalisées, le développement de mesures calculées avec MDX et l'utilisation de requêtes SQL pour produire des rapports décisionnels et étudier les stratégies d'agrégation.

## Objectifs du projet

- Comprendre l'architecture et le fonctionnement d'un Data Warehouse.
- Créer, déployer et explorer un cube OLAP avec SSAS.
- Analyser les ventes selon plusieurs dimensions.
- Étudier les relations entre les clients, les promotions et les performances commerciales.
- Réaliser une analyse temporelle pour identifier les tendances et les variations saisonnières.
- Concevoir des vues analytiques géographiques et par type de produit.
- Créer des mesures calculées avec MDX.
- Explorer les techniques d'agrégation `CUBE` et `ROLLUP`.
- Rédiger des requêtes SQL OLAP et comparer SQL relationnel et analyse multidimensionnelle.

## Technologies et outils utilisés

| Technologie | Utilisation |
|---|---|
| Microsoft SQL Server | Gestion de la base de données |
| SQL Server Analysis Services (SSAS) | Création et exploitation des cubes OLAP |
| Visual Studio 2022 | Développement du projet et modélisation du cube |
| SQL Server Management Studio (SSMS 21) | Administration et exécution des requêtes SQL/MDX |
| SQL | Requêtes relationnelles et rapports analytiques |
| MDX | Calculs multidimensionnels et analyses OLAP |
| Adventure Works DW2022 | Base de données Data Warehouse utilisée |

---

# Réalisation du projet

## 1. Modélisation multidimensionnelle initiale et analyse des ventes

### 1.1. Configuration de la source de données

La première étape a consisté à créer un projet Analysis Services dans Visual Studio 2022 et à configurer une source de données connectée à la base `Adventure Works DW2022`.

Opérations principales :
- Création d'un projet Analysis Services.
- Ajout d'une source de données.
- Configuration de la connexion SQL Server.
- Sélection de la base `Adventure Works DW2022`.
- Test et validation de la connexion.

### 1.2. Création de la Data Source View

Une Data Source View (DSV) a été créée afin de fournir une représentation logique des tables nécessaires au cube.

Le modèle initial comprend la table de faits centrale :
- `FactInternetSales`

Et les principales dimensions :
- `DimCustomer`
- `DimDate`
- `DimProduct`
- `DimPromotion`

La DSV permet de visualiser les tables et leurs relations avec la table de faits.

### 1.3. Création, déploiement et traitement du cube OLAP

Un cube OLAP a été créé à partir de la Data Source View, en utilisant `FactInternetSales` comme table de faits centrale.

Après la configuration de sa structure, le cube a été déployé sur le serveur Analysis Services et traité afin de rendre les données disponibles pour l'exploration.

L'interface Browse a ensuite été utilisée pour explorer les mesures et les dimensions selon différentes perspectives analytiques.

### 1.4. Tâche 1 – Analyse croisée des ventes par client et promotion

**Objectif :** Analyser les ventes par client et étudier la contribution des campagnes promotionnelles.

**Analyse des clients :** La dimension `DimCustomer` a été explorée à travers l'interface Browse du cube. Les attributs des clients, notamment les clés clients et les prénoms, ont été utilisés pour afficher les montants des ventes par client.

**Analyse des promotions :** La dimension `DimPromotion` a été intégrée à l'analyse. Les attributs étudiés comprennent :
- `English Promotion Name`
- `English Promotion Type`
- `Promotion Key`

Les montants des ventes ont été examinés selon les clients et les promotions afin de comparer les offres commerciales et leur répartition.

**Interprétation et considérations métiers :**
- Une part importante des ventes peut provenir d'achats sans remise.
- Les promotions basées sur les volumes peuvent contribuer à l'activité commerciale.
- Les promotions de liquidation peuvent augmenter les volumes tout en exerçant une pression sur la rentabilité.

**Recommandations :**
- Privilégier les promotions ciblées plutôt que les remises généralisées.
- Évaluer les promotions à partir de leur rentabilité et pas uniquement du volume des ventes.
- Analyser le comportement d'achat des clients avant de lancer des campagnes.
- Surveiller l'effet des réductions sur les marges bénéficiaires.

## 2. Analyse temporelle avancée

### 2.1. Intégration de la dimension Date

La dimension `DimDate` a été utilisée pour réaliser une analyse temporelle. L'analyse est organisée selon les niveaux :
- Calendar Year
- Calendar Quarter
- Month

L'attribut `English Month Name` permet d'afficher les ventes mensuelles. Le classement des mois a été configuré à partir de `MonthNumberOfYear` afin de garantir un ordre chronologique plutôt qu'alphabétique.

### 2.2. Création d'une mesure calculée avec MDX

Une mesure calculée nommée `Variation Ventes` a été créée pour calculer la différence entre les ventes de la période courante et celles de la période précédente.

```mdx
[Measures].[Sales Amount] -
(
    [Measures].[Sales Amount],
    [Dim Date].[Hierarchie].CurrentMember.PrevMember
)
```

Cette expression compare le montant des ventes du membre courant avec celui du membre précédent dans la hiérarchie temporelle.

### 2.3. Exploration et interprétation temporelles

Le cube a été exploré aux niveaux année, trimestre et mois. La mesure calculée a permis d'observer les variations entre les périodes et d'identifier les hausses et les baisses des ventes.

**Recommandations :**
- Anticiper les fluctuations saisonnières de la demande.
- Adapter les niveaux de stock aux ventes prévisionnelles.
- Programmer les campagnes marketing pendant les périodes de forte activité.
- Étudier les périodes de baisse des ventes.
- Exploiter les tendances historiques pour améliorer les prévisions.

## 3. Création de vues analytiques complexes

Cette partie enrichit le modèle initial afin de permettre des analyses géographiques et des analyses par type de produit.

Les dimensions supplémentaires comprennent :
- `DimGeography`
- `DimProductCategory`
- `DimProductSubcategory`

### 3.1. Vue 1 – Analyse des ventes par région

**Objectif :** Analyser les ventes selon les régions géographiques et les catégories de produits.

**Attributs géographiques :** La dimension `DimGeography` fournit notamment les attributs :
- City
- State Province
- Country Region
- Geography Key

**Relation entre dimensions :** Les informations géographiques étant liées aux clients par `GeographyKey`, une relation de type référencé a été configurée entre `DimCustomer` et `DimGeography`, à l'aide de `GeographyKey`.

Cette relation permet d'utiliser les attributs géographiques pour analyser les mesures issues de `FactInternetSales`.

**Hiérarchie des produits :** L'analyse des produits a été étendue à travers :
- `DimProduct`
- `DimProductSubcategory`
- `DimProductCategory`

La relation entre `DimProduct` et `DimProductSubcategory` utilise `ProductSubcategoryKey`. Cette hiérarchie permet d'analyser les ventes au niveau du produit, de la sous-catégorie et de la catégorie.

**Interprétation et recommandations :**
- Identifier les zones géographiques générant les ventes les plus importantes.
- Adapter la disponibilité des produits à la demande régionale.
- Améliorer la répartition des stocks.
- Développer des campagnes marketing fondées sur les comportements d'achat régionaux.

### 3.2. Vue 2 – Analyse des ventes par type de produit

**Objectif :** Analyser les ventes selon les types de produits et les profils des clients.

Le cube a été exploré en combinant les attributs des produits et les informations clients, notamment :
- Catégories et sous-catégories de produits
- `English Product Name`
- `Customer First Name`
- `English Occupation`

Les montants des ventes ont été étudiés au niveau des produits, avec les mesures `Total Product Cost` et `Order Quantity`.

**Mesure calculée – Coût moyen par produit**

Une mesure calculée nommée `Coût Moyen par Produit` a été créée :

```mdx
[Measures].[Total Product Cost] /
[Measures].[Order Quantity]
```

Elle calcule le coût moyen d'un produit à partir du coût total des produits et de la quantité commandée.

**Mesure calculée – Coût moyen par vente**

Une deuxième mesure calculée nommée `Coût Moyen par Vente` a été créée :

```mdx
[Measures].[Total Product Cost] /
[Measures].[Fact Internet Sales Count]
```

Elle estime le coût moyen par transaction à partir du coût total des produits et de la mesure du nombre de ventes.

**Interprétation et recommandations :**
- Identifier les produits présentant des coûts moyens élevés.
- Comparer les coûts des produits selon les segments de clientèle.
- Améliorer la gestion du portefeuille de produits.
- Suivre les indicateurs de coûts en complément du chiffre d'affaires.
- Exploiter les profils clients pour améliorer les recommandations de produits et les campagnes marketing.

## 4. Optimisation des agrégations et comparaison des performances

Cette partie porte sur les stratégies d'agrégation et l'utilisation de SQL et MDX pour l'analyse décisionnelle.

### 4.1. Agrégations SQL avancées : ROLLUP et CUBE

**ROLLUP** génère des sous-totaux hiérarchiques ainsi qu'un total général. Il est utile pour les rapports géographiques, par exemple les ventes par pays et par État.

Exemple illustratif :

```sql
SELECT
    g.EnglishCountryRegionName AS Country,
    g.StateProvinceName AS State,
    SUM(f.SalesAmount) AS TotalSales
FROM FactInternetSales f
JOIN DimCustomer c
    ON f.CustomerKey = c.CustomerKey
JOIN DimGeography g
    ON c.GeographyKey = g.GeographyKey
GROUP BY ROLLUP (
    g.EnglishCountryRegionName,
    g.StateProvinceName
);
```

**CUBE** génère les agrégations pour toutes les combinaisons possibles des dimensions sélectionnées. Il est utile pour comparer les ventes selon les régions géographiques et les catégories de produits.

Exemple illustratif :

```sql
SELECT
    g.EnglishCountryRegionName AS Country,
    pc.EnglishProductCategoryName AS Category,
    SUM(f.SalesAmount) AS TotalSales
FROM FactInternetSales f
JOIN DimCustomer c
    ON f.CustomerKey = c.CustomerKey
JOIN DimGeography g
    ON c.GeographyKey = g.GeographyKey
JOIN DimProduct p
    ON f.ProductKey = p.ProductKey
JOIN DimProductSubcategory ps
    ON p.ProductSubcategoryKey = ps.ProductSubcategoryKey
JOIN DimProductCategory pc
    ON ps.ProductCategoryKey = pc.ProductCategoryKey
GROUP BY CUBE (
    g.EnglishCountryRegionName,
    pc.EnglishProductCategoryName
);
```

### 4.2. Requêtes SQL OLAP

Le cahier des charges du projet prévoit trois thèmes de requêtes analytiques SQL.

**Requête 1 – Top 10 des promotions par ventes**

Objectif : Identifier les promotions générant les montants de ventes les plus élevés.

```sql
SELECT TOP 10
    p.EnglishPromotionName,
    SUM(f.SalesAmount) AS TotalSales
FROM FactInternetSales f
JOIN DimPromotion p
    ON f.PromotionKey = p.PromotionKey
GROUP BY p.EnglishPromotionName
ORDER BY TotalSales DESC;
```

**Requête 2 – Répartition des ventes par région avec sous-totaux**

Objectif : Calculer les ventes par région géographique, avec les sous-totaux et le total général.

```sql
SELECT
    g.EnglishCountryRegionName AS Country,
    g.StateProvinceName AS State,
    SUM(f.SalesAmount) AS TotalSales
FROM FactInternetSales f
JOIN DimCustomer c
    ON f.CustomerKey = c.CustomerKey
JOIN DimGeography g
    ON c.GeographyKey = g.GeographyKey
GROUP BY ROLLUP (
    g.EnglishCountryRegionName,
    g.StateProvinceName
)
ORDER BY Country, State;
```

**Requête 3 – Évolution mensuelle des ventes**

Objectif : Analyser l'évolution mensuelle des ventes sur une période sélectionnée.

```sql
SELECT
    d.CalendarYear,
    d.CalendarQuarter,
    d.MonthNumberOfYear,
    d.EnglishMonthName,
    SUM(f.SalesAmount) AS MonthlySales
FROM FactInternetSales f
JOIN DimDate d
    ON f.OrderDateKey = d.DateKey
WHERE d.CalendarYear BETWEEN 2012 AND 2013
GROUP BY
    d.CalendarYear,
    d.CalendarQuarter,
    d.MonthNumberOfYear,
    d.EnglishMonthName
ORDER BY
    d.CalendarYear,
    d.MonthNumberOfYear;
```

La période peut être adaptée selon les besoins de l'analyse.

### 4.3. Analyse MDX – Top 10 des promotions

L'analyse des meilleures promotions a également été exprimée en MDX. La fonction `TOPCOUNT` sélectionne les dix membres de la dimension Promotion ayant les montants de ventes les plus élevés.

```mdx
SELECT
{
    [Measures].[Sales Amount]
} ON COLUMNS,

TOPCOUNT(
    [Dim Promotion].[English Promotion Name]
        .[English Promotion Name].MEMBERS,
    10,
    [Measures].[Sales Amount]
) ON ROWS

FROM [tache-1]
```

Cette requête exploite la structure multidimensionnelle du cube et ses mesures.

### 4.4. Comparaison des performances SQL et OLAP

La comparaison porte sur :
- Le temps d'exécution des requêtes.
- Les stratégies d'agrégation.
- L'organisation des données.
- Le calcul des sous-totaux.
- La réutilisation des mesures analytiques pré-calculées.

Les requêtes SQL relationnelles peuvent effectuer des jointures, regroupements, tris et agrégations lors de leur exécution. Un cube OLAP peut exploiter sa structure multidimensionnelle et les agrégations configurées pour faciliter les requêtes analytiques répétitives.

**Recommandations d'optimisation :**
- Identifier les rapports les plus fréquemment demandés.
- Configurer des agrégations adaptées aux besoins analytiques.
- Optimiser les relations entre dimensions et les hiérarchies.
- Éviter les jointures et les calculs inutiles.
- Utiliser des mesures pré-calculées lorsque cela est pertinent.
- Comparer les temps d'exécution dans des conditions équivalentes.

---

## Conclusion générale

Ce mini-projet a permis de mettre en pratique la conception, le développement et l'exploitation d'un Data Warehouse multidimensionnel avec Microsoft SQL Server Analysis Services.

Les travaux ont couvert l'analyse des clients et des promotions, l'analyse temporelle, la création de vues analytiques complexes, les mesures calculées en MDX, les requêtes SQL OLAP et l'optimisation des agrégations.

Le projet montre comment les cubes OLAP peuvent transformer des données transactionnelles en informations analytiques structurées utiles à la Business Intelligence et à la prise de décision.

## Compétences acquises

- Architecture Data Warehouse
- Modélisation multidimensionnelle
- Conception et déploiement de cubes OLAP
- SQL Server Analysis Services (SSAS)
- SQL et MDX
- Business Intelligence
- Analyse des données
- Analyse temporelle et géographique
- Mesures calculées
- Agrégations SQL : CUBE et ROLLUP
- Optimisation des requêtes et des agrégations

## Contexte académique

- **Établissement :** École Supérieure de Technologie d'El Kelâa des Sraghna (ESTK), Université Cadi Ayyad
- **Formation :** Deuxième année DUT – Ingénierie des Données
- **Module :** Data Warehouse
- **Année universitaire :** 2025–2026
- **Projet :** Analyse et Personnalisation de Cubes OLAP dans SSAS
