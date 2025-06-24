# Syntaxe SQL - Guide Complet

## Introduction à la Syntaxe SQL

SQL utilise des **mots-clés** (keywords) en anglais pour construire des instructions. Ces mots-clés sont généralement écrits en MAJUSCULES par convention, bien que SQL soit insensible à la casse.

### Règles de Base
- Chaque instruction se termine par un **point-virgule (;)**
- Les commentaires utilisent `--` (une ligne) ou `/* */` (multi-lignes)
- Les espaces et retours à la ligne sont ignorés (formatage libre)

## Vocabulaire SQL Essentiel

### Mots-Clés de Structure
- **DATABASE** : Base de données
- **SCHEMA** : Schéma (organisation logique)
- **TABLE** : Table
- **COLUMN** / **FIELD** : Colonne/Champ
- **ROW** / **RECORD** : Ligne/Enregistrement
- **INDEX** : Index (optimisation)
- **VIEW** : Vue (requête sauvegardée)

### Mots-Clés d'Action
- **CREATE** : Créer
- **ALTER** : Modifier
- **DROP** : Supprimer définitivement
- **INSERT** : Insérer
- **SELECT** : Sélectionner/Lire
- **UPDATE** : Mettre à jour
- **DELETE** : Supprimer des lignes

### Mots-Clés de Condition
- **WHERE** : Condition de filtrage
- **HAVING** : Condition sur groupes
- **AND** : ET logique
- **OR** : OU logique
- **NOT** : NON logique
- **IN** : Dans une liste
- **BETWEEN** : Entre deux valeurs
- **LIKE** : Comparaison avec motif
- **IS NULL** / **IS NOT NULL** : Test de valeur nulle

### Mots-Clés d'Organisation
- **ORDER BY** : Tri des résultats
- **GROUP BY** : Regroupement
- **LIMIT** : Limitation du nombre de résultats
- **DISTINCT** : Valeurs uniques seulement

## Types de Données SQL

### Types Numériques
```sql
INTEGER         -- Entier (-2,147,483,648 à 2,147,483,647)
BIGINT          -- Grand entier
SMALLINT        -- Petit entier (-32,768 à 32,767)
DECIMAL(m,n)    -- Décimal exact (m chiffres, n après virgule)
NUMERIC(m,n)    -- Synonyme de DECIMAL
REAL            -- Nombre flottant simple précision
DOUBLE          -- Nombre flottant double précision
SERIAL          -- Entier auto-incrémenté (PostgreSQL)
AUTO_INCREMENT  -- Entier auto-incrémenté (MySQL)
```

### Types Texte
```sql
CHAR(n)         -- Chaîne de longueur fixe (n caractères)
VARCHAR(n)      -- Chaîne de longueur variable (max n caractères)
TEXT            -- Texte long sans limite
```

### Types Date/Heure
```sql
DATE            -- Date (YYYY-MM-DD)
TIME            -- Heure (HH:MM:SS)
TIMESTAMP       -- Date et heure complètes
DATETIME        -- Date et heure (MySQL)
```

### Types Booléens
```sql
BOOLEAN         -- TRUE/FALSE
BOOL            -- Synonyme de BOOLEAN
```

## Opérateurs SQL

### Opérateurs de Comparaison
```sql
=               -- Égal
!=              -- Différent
>               -- Supérieur
>=              -- Supérieur ou égal
<               -- Inférieur
<=              -- Inférieur ou égal
```

### Opérateurs de Motif (LIKE)
```sql
%               -- N'importe quelle séquence de caractères
_               -- Un seul caractère
-- Exemples :
'Jean%'         -- Commence par "Jean"
'%martin%'      -- Contient "martin"
'J_an'          -- "Jean", "Joan", etc.
```

### Opérateurs Logiques
```sql
AND             -- ET logique
OR              -- OU logique
NOT             -- NON logique
```

## DDL - Data Definition Language

### Gestion des Bases de Données
```sql
-- Créer une base de données
CREATE DATABASE nom_base;
CREATE DATABASE ecommerce;

-- Utiliser une base de données
USE nom_base;
USE ecommerce;

-- Supprimer une base de données
DROP DATABASE nom_base;
```

### Création de Tables
```sql
-- Syntaxe générale
CREATE TABLE nom_table (
    colonne1 TYPE [CONTRAINTES],
    colonne2 TYPE [CONTRAINTES],
    -- ...
    [CONTRAINTES_TABLE]
);

-- Exemple complet
CREATE TABLE clients (
    id SERIAL PRIMARY KEY,
    nom VARCHAR(100) NOT NULL,
    prenom VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE NOT NULL,
    telephone VARCHAR(20),
    date_naissance DATE,
    adresse TEXT,
    ville VARCHAR(50) DEFAULT 'Paris',
    code_postal CHAR(5),
    date_creation TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    actif BOOLEAN DEFAULT TRUE
);
```

### Contraintes de Colonnes
```sql
NOT NULL        -- Valeur obligatoire
UNIQUE          -- Valeur unique dans la table
PRIMARY KEY     -- Clé primaire (unique + not null)
DEFAULT valeur  -- Valeur par défaut
CHECK (condition) -- Condition à respecter
```

### Contraintes de Tables
```sql
-- Clé primaire composite
PRIMARY KEY (colonne1, colonne2)

-- Clé étrangère
FOREIGN KEY (colonne) REFERENCES autre_table(colonne)

-- Contrainte de vérification
CHECK (condition)

-- Exemple avec contraintes
CREATE TABLE commandes (
    id SERIAL PRIMARY KEY,
    numero VARCHAR(20) UNIQUE NOT NULL,
    client_id INTEGER NOT NULL,
    date_commande TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    total DECIMAL(10,2) CHECK (total >= 0),
    statut VARCHAR(20) DEFAULT 'en_attente',
    FOREIGN KEY (client_id) REFERENCES clients(id),
    CHECK (statut IN ('en_attente', 'validee', 'expediee', 'livree'))
);
```

### Modification de Tables
```sql
-- Ajouter une colonne
ALTER TABLE nom_table ADD COLUMN nouvelle_colonne TYPE;
ALTER TABLE clients ADD COLUMN newsletter BOOLEAN DEFAULT FALSE;

-- Modifier une colonne
ALTER TABLE nom_table ALTER COLUMN nom_colonne TYPE nouveau_type;
ALTER TABLE clients ALTER COLUMN telephone TYPE VARCHAR(30);

-- Renommer une colonne
ALTER TABLE nom_table RENAME COLUMN ancien_nom TO nouveau_nom;

-- Supprimer une colonne
ALTER TABLE nom_table DROP COLUMN nom_colonne;
ALTER TABLE clients DROP COLUMN newsletter;

-- Ajouter une contrainte
ALTER TABLE nom_table ADD CONSTRAINT nom_contrainte CHECK (condition);

-- Supprimer une contrainte
ALTER TABLE nom_table DROP CONSTRAINT nom_contrainte;
```

### Suppression de Tables
```sql
-- Supprimer une table
DROP TABLE nom_table;
DROP TABLE clients;

-- Supprimer si elle existe
DROP TABLE IF EXISTS nom_table;
```

## DML - Data Manipulation Language

### INSERT - Insertion de Données
```sql
-- Insertion simple
INSERT INTO nom_table (colonne1, colonne2) VALUES (valeur1, valeur2);

-- Exemple
INSERT INTO clients (nom, prenom, email) 
VALUES ('Dupont', 'Jean', 'jean.dupont@email.com');

-- Insertion multiple
INSERT INTO clients (nom, prenom, email) VALUES 
('Martin', 'Paul', 'paul.martin@email.com'),
('Durand', 'Marie', 'marie.durand@email.com'),
('Bernard', 'Luc', 'luc.bernard@email.com');

-- Insertion avec toutes les colonnes (dans l'ordre de création)
INSERT INTO clients VALUES 
(DEFAULT, 'Moreau', 'Anne', 'anne.moreau@email.com', '0123456789', 
 '1985-03-15', '123 rue de la Paix', 'Lyon', '69000', DEFAULT, DEFAULT);

-- Insertion depuis une autre table
INSERT INTO clients_archives (nom, prenom, email)
SELECT nom, prenom, email FROM clients WHERE actif = FALSE;
```

### SELECT - Lecture de Données

#### Syntaxe Complète
```sql
SELECT [DISTINCT] colonnes
FROM table1
[JOIN table2 ON condition]
[WHERE conditions]
[GROUP BY colonnes]
[HAVING conditions_groupes]
[ORDER BY colonnes [ASC|DESC]]
[LIMIT nombre [OFFSET decalage]];
```

#### Sélections de Base
```sql
-- Toutes les colonnes
SELECT * FROM clients;

-- Colonnes spécifiques
SELECT nom, prenom, email FROM clients;

-- Avec alias de colonnes
SELECT nom AS nom_famille, prenom AS nom_prenom FROM clients;

-- Valeurs distinctes uniquement
SELECT DISTINCT ville FROM clients;

-- Calculs dans SELECT
SELECT nom, prenom, (2024 - EXTRACT(YEAR FROM date_naissance)) AS age 
FROM clients;
```

#### Filtrage avec WHERE
```sql
-- Conditions simples
SELECT * FROM clients WHERE ville = 'Paris';
SELECT * FROM clients WHERE age > 30;

-- Conditions multiples
SELECT * FROM clients WHERE ville = 'Paris' AND age > 25;
SELECT * FROM clients WHERE ville = 'Paris' OR ville = 'Lyon';

-- Négation
SELECT * FROM clients WHERE NOT ville = 'Paris';
SELECT * FROM clients WHERE ville != 'Paris';

-- Plages de valeurs
SELECT * FROM clients WHERE age BETWEEN 25 AND 40;
SELECT * FROM clients WHERE ville IN ('Paris', 'Lyon', 'Marseille');

-- Motifs avec LIKE
SELECT * FROM clients WHERE nom LIKE 'Dup%';
SELECT * FROM clients WHERE email LIKE '%@gmail.com';

-- Valeurs nulles
SELECT * FROM clients WHERE telephone IS NULL;
SELECT * FROM clients WHERE telephone IS NOT NULL;
```

#### Tri avec ORDER BY
```sql
-- Tri croissant (par défaut)
SELECT * FROM clients ORDER BY nom;
SELECT * FROM clients ORDER BY nom ASC;

-- Tri décroissant
SELECT * FROM clients ORDER BY age DESC;

-- Tri multiple
SELECT * FROM clients ORDER BY ville ASC, nom ASC;
SELECT * FROM clients ORDER BY age DESC, nom ASC;
```

#### Limitation avec LIMIT
```sql
-- Premiers résultats
SELECT * FROM clients LIMIT 10;

-- Pagination avec OFFSET
SELECT * FROM clients ORDER BY id LIMIT 10 OFFSET 20; -- Page 3 (20-30)
```

### UPDATE - Modification de Données
```sql
-- Syntaxe générale
UPDATE nom_table 
SET colonne1 = valeur1, colonne2 = valeur2
WHERE condition;

-- Exemples
UPDATE clients SET telephone = '0987654321' WHERE id = 1;

UPDATE clients 
SET ville = 'Marseille', code_postal = '13000' 
WHERE nom = 'Dupont' AND prenom = 'Jean';

-- Mise à jour avec calculs
UPDATE produits SET prix = prix * 1.1 WHERE categorie = 'electronique';

-- Mise à jour depuis une autre table
UPDATE clients SET ville = (
    SELECT ville FROM adresses WHERE adresses.client_id = clients.id
) WHERE EXISTS (
    SELECT 1 FROM adresses WHERE adresses.client_id = clients.id
);
```

### DELETE - Suppression de Données
```sql
-- Syntaxe générale
DELETE FROM nom_table WHERE condition;

-- Exemples
DELETE FROM clients WHERE id = 1;
DELETE FROM clients WHERE actif = FALSE;
DELETE FROM clients WHERE date_creation < '2020-01-01';

-- Supprimer toutes les lignes (attention !)
DELETE FROM nom_table;

-- Alternative plus rapide pour vider une table
TRUNCATE TABLE nom_table;
```

## Fonctions d'Agrégation

### Fonctions de Base
```sql
COUNT(*)            -- Nombre total de lignes
COUNT(colonne)      -- Nombre de valeurs non-nulles
COUNT(DISTINCT col) -- Nombre de valeurs distinctes
SUM(colonne)        -- Somme
AVG(colonne)        -- Moyenne
MIN(colonne)        -- Minimum
MAX(colonne)        -- Maximum
```

### Exemples d'Utilisation
```sql
-- Statistiques simples
SELECT COUNT(*) AS nombre_clients FROM clients;
SELECT AVG(age) AS age_moyen FROM clients;
SELECT MIN(date_creation) AS premier_client FROM clients;

-- Regroupements
SELECT ville, COUNT(*) AS nombre_clients 
FROM clients 
GROUP BY ville;

SELECT ville, AVG(age) AS age_moyen 
FROM clients 
GROUP BY ville 
HAVING COUNT(*) > 5;
```

## Jointures (JOINS)

### Types de Jointures
```sql
-- INNER JOIN : Seulement les correspondances
SELECT c.nom, cmd.date_commande
FROM clients c
INNER JOIN commandes cmd ON c.id = cmd.client_id;

-- LEFT JOIN : Tous les enregistrements de gauche
SELECT c.nom, cmd.date_commande
FROM clients c
LEFT JOIN commandes cmd ON c.id = cmd.client_id;

-- RIGHT JOIN : Tous les enregistrements de droite
SELECT c.nom, cmd.date_commande
FROM clients c
RIGHT JOIN commandes cmd ON c.id = cmd.client_id;

-- FULL OUTER JOIN : Tous les enregistrements des deux tables
SELECT c.nom, cmd.date_commande
FROM clients c
FULL OUTER JOIN commandes cmd ON c.id = cmd.client_id;
```

### Jointures Multiples
```sql
SELECT c.nom, p.nom_produit, dc.quantite
FROM clients c
INNER JOIN commandes cmd ON c.id = cmd.client_id
INNER JOIN details_commandes dc ON cmd.id = dc.commande_id
INNER JOIN produits p ON dc.produit_id = p.id;
```

## Sous-Requêtes (Subqueries)

### Dans WHERE
```sql
-- Clients ayant passé des commandes
SELECT * FROM clients 
WHERE id IN (SELECT DISTINCT client_id FROM commandes);

-- Clients n'ayant jamais commandé
SELECT * FROM clients 
WHERE id NOT IN (SELECT client_id FROM commandes WHERE client_id IS NOT NULL);
```

### Dans SELECT
```sql
SELECT nom, prenom,
       (SELECT COUNT(*) FROM commandes WHERE client_id = clients.id) AS nb_commandes
FROM clients;
```

Ce guide couvre la syntaxe SQL de base nécessaire pour manipuler efficacement les bases de données relationnelles, de la création des structures à la manipulation des données.