# 📦 SPÉCIFICATIONS FONCTIONNELLES - MODULE DE GESTION DE STOCK

> **Version:** 2.0  
> **Type:** Spécifications fonctionnelles ERP  
> **Dernière mise à jour:** Janvier 2026

---

## 📋 Table des matières

1. [Vue d'ensemble](#1-vue-densemble)
2. [Fonctionnalités du module](#2-fonctionnalités-du-module)
3. [Actions utilisateurs détaillées](#3-actions-utilisateurs-détaillées)
4. [Matrice des permissions](#4-matrice-des-permissions)
5. [Rôles types et attributions](#5-rôles-types-et-attributions)
6. [Annexes](#6-annexes)

---

## 1. VUE D'ENSEMBLE

### 1.1 Objectif du module

Le module de gestion de stock permet de :
- Gérer le référentiel articles et entrepôts
- Suivre les mouvements de stock en temps réel
- Assurer la traçabilité complète des produits
- Optimiser les niveaux de stock et réapprovisionnements
- Produire des rapports et analyses décisionnelles

### 1.2 Périmètre fonctionnel

```
┌─────────────────────────────────────────────────────────────────┐
│                    MODULE GESTION DE STOCK                       │
├─────────────┬─────────────┬─────────────┬─────────────┬─────────┤
│ RÉFÉRENTIEL │ OPÉRATIONS  │ INVENTAIRES │ TRAÇABILITÉ │ REPORTS │
├─────────────┼─────────────┼─────────────┼─────────────┼─────────┤
│ • Articles  │ • Entrées   │ • Tournant  │ • Par lot   │ • Valori│
│ • Familles  │ • Sorties   │ • Annuel    │ • Par série │   sation│
│ • Entrepôts │ • Transferts│ • Partiel   │ • Historique│ • KPIs  │
│ • Emplacem. │ • Ajustement│ • Comptage  │ • Audit     │ • Export│
│ • Unités    │ • Réservat. │             │             │         │
└─────────────┴─────────────┴─────────────┴─────────────┴─────────┘
```

---

## 2. FONCTIONNALITÉS DU MODULE

### 2.1 Configuration & Référentiel

#### A. Gestion des articles / produits

| ID | Fonctionnalité | Description | Priorité |
|----|----------------|-------------|----------|
| REF-001 | Gestion des articles | Création, modification, archivage des fiches articles | Critique |
| REF-002 | Familles & catégories | Organisation hiérarchique des produits | Haute |
| REF-003 | Sous-catégories | Classification détaillée multi-niveaux | Moyenne |
| REF-004 | Unités de mesure (UoM) | Définition des unités et conversions | Critique |
| REF-005 | Variantes produit | Gestion des déclinaisons (taille, couleur...) | Moyenne |

#### B. Gestion des infrastructures

| ID | Fonctionnalité | Description | Priorité |
|----|----------------|-------------|----------|
| INF-001 | Entrepôts / Magasins | Création et paramétrage des sites de stockage | Critique |
| INF-002 | Zones d'entreposage | Découpage logique (réception, picking, réserve, expédition) | Haute |
| INF-003 | Emplacements | Adressage précis des stocks (allée, rack, niveau, case) | Haute |
| INF-004 | Multi-sites | Gestion de plusieurs entrepôts distincts | Moyenne |

#### C. Paramétrage avancé

| ID | Fonctionnalité | Description | Priorité |
|----|----------------|-------------|----------|
| PAR-001 | Statuts de stock | Disponible, réservé, bloqué, non conforme, en quarantaine | Critique |
| PAR-002 | Niveaux de stock | Seuils min, max, sécurité, réapprovisionnement | Haute |
| PAR-003 | Codes-barres / QR / RFID | Configuration des identifiants de scan | Haute |
| PAR-004 | Numéros de série | Suivi unitaire des articles sérialisés | Moyenne |
| PAR-005 | Gestion des lots | Regroupement et traçabilité par lot | Haute |
| PAR-006 | Méthodes de valorisation | FIFO, LIFO, CUMP, prix standard | Critique |
| PAR-007 | Réapprovisionnement auto | Règles de commande automatique | Moyenne |
| PAR-008 | Dates de péremption | Gestion DLC/DLUO | Haute |

---

### 2.2 Gestion opérationnelle du stock

#### A. Mouvements d'entrée

| ID | Fonctionnalité | Description | Priorité |
|----|----------------|-------------|----------|
| ENT-001 | Entrées manuelles | Saisie directe des entrées en stock | Critique |
| ENT-002 | Réceptions d'achats | Entrées liées aux commandes fournisseurs | Critique |
| ENT-003 | Retours clients | Réintégration des produits retournés | Haute |
| ENT-004 | Productions | Entrées issues de la fabrication | Moyenne |
| ENT-005 | Entrées d'inventaire | Régularisation positive | Haute |

#### B. Mouvements de sortie

| ID | Fonctionnalité | Description | Priorité |
|----|----------------|-------------|----------|
| SOR-001 | Sorties manuelles | Déstockage direct | Critique |
| SOR-002 | Expéditions clients | Sorties liées aux commandes clients | Critique |
| SOR-003 | Retours fournisseurs | Renvoi de marchandises | Haute |
| SOR-004 | Consommations | Sorties pour usage interne | Moyenne |
| SOR-005 | Pertes / Casse | Sorties pour destruction | Haute |
| SOR-006 | Sorties d'inventaire | Régularisation négative | Haute |

#### C. Transferts et ajustements

| ID | Fonctionnalité | Description | Priorité |
|----|----------------|-------------|----------|
| TRF-001 | Transferts inter-entrepôts | Mouvement entre sites | Haute |
| TRF-002 | Transferts internes | Mouvement entre zones/emplacements | Haute |
| TRF-003 | Ajustements de stock | Corrections d'écarts | Critique |
| TRF-004 | Réservations | Blocage de quantités pour commandes | Haute |
| TRF-005 | Libération de réservation | Déblocage des quantités réservées | Haute |
| TRF-006 | Mise en quarantaine | Blocage qualité | Moyenne |
| TRF-007 | Changement de statut | Modification de l'état du stock | Haute |

---

### 2.3 Inventaires

| ID | Fonctionnalité | Description | Priorité |
|----|----------------|-------------|----------|
| INV-001 | Inventaire tournant | Comptage cyclique par rotation | Haute |
| INV-002 | Inventaire annuel | Comptage complet obligatoire | Critique |
| INV-003 | Inventaire partiel | Par article, zone, famille ou magasin | Haute |
| INV-004 | Comptage physique | Saisie des quantités réelles | Critique |
| INV-005 | Gestion des écarts | Analyse et justification des différences | Critique |
| INV-006 | Validation & ajustement | Application automatique des corrections | Critique |
| INV-007 | Gel de stock | Blocage des mouvements pendant inventaire | Haute |
| INV-008 | Double comptage | Vérification par second opérateur | Moyenne |

---

### 2.4 Suivi & Traçabilité

| ID | Fonctionnalité | Description | Priorité |
|----|----------------|-------------|----------|
| TRA-001 | Historique des mouvements | Journal complet de toutes les opérations | Critique |
| TRA-002 | Traçabilité par lot | Suivi de l'origine à la destination | Critique |
| TRA-003 | Traçabilité par série | Suivi unitaire des articles | Haute |
| TRA-004 | Traçabilité descendante | Expédition → Articles → Lots → Composants | Haute |
| TRA-005 | Traçabilité ascendante | Lot → Origine → Fournisseur → Matières | Haute |
| TRA-006 | Audit utilisateur | Qui a fait quoi et quand | Critique |
| TRA-007 | Piste d'audit | Conservation des modifications | Critique |

---

### 2.5 Analyses & Reporting

| ID | Fonctionnalité | Description | Priorité |
|----|----------------|-------------|----------|
| REP-001 | Valorisation du stock | Par entrepôt, famille, article | Critique |
| REP-002 | État des stocks | Quantités en temps réel | Critique |
| REP-003 | Rapport des écarts | Analyse des différences d'inventaire | Haute |
| REP-004 | Rotation de stock | Indicateurs de performance | Haute |
| REP-005 | Alertes ruptures | Notification des seuils atteints | Critique |
| REP-006 | Prévisions de stock | Anticipation des besoins | Moyenne |
| REP-007 | Performance logistique | KPIs opérationnels | Moyenne |
| REP-008 | Analyse des coûts | Suivi des marges et coûts | Haute |
| REP-009 | Tableaux de bord | Synthèse visuelle des indicateurs | Haute |
| REP-010 | Export données | PDF, Excel, CSV | Critique |

---

## 3. ACTIONS UTILISATEURS DÉTAILLÉES

### 3.1 Gestion des articles

| Action | Description | Pré-requis | Résultat attendu |
|--------|-------------|------------|------------------|
| Créer un article | Saisie fiche complète | Permission `article.create` | Article actif créé |
| Modifier un article | Mise à jour des informations | Permission `article.update` | Fiche mise à jour |
| Archiver un article | Désactivation sans suppression | Permission `article.archive` | Article inactif |
| Supprimer un article | Suppression définitive | Permission `article.delete` + stock = 0 | Article supprimé |
| Activer/Désactiver | Changement de statut | Permission `article.update` | Statut modifié |
| Ajouter une image | Upload photo produit | Permission `article.update` | Image associée |
| Configurer prix/taxes | Paramétrage commercial | Permission `article.pricing` | Prix définis |
| Associer UoM | Lier unité de mesure | Permission `article.update` | UoM associée |
| Définir seuils stock | Min/Max/Sécurité | Permission `article.update` | Seuils enregistrés |
| Configurer sérialisation | Activer lots/séries | Permission `article.update` | Mode traçabilité actif |
| Dupliquer un article | Copie d'une fiche existante | Permission `article.create` | Nouvel article créé |
| Importer articles | Import en masse | Permission `article.import` | Articles créés |
| Exporter articles | Export de la liste | Permission `article.export` | Fichier généré |

### 3.2 Gestion des entrepôts & emplacements

| Action | Description | Pré-requis | Résultat attendu |
|--------|-------------|------------|------------------|
| Créer un entrepôt | Définition d'un nouveau site | Permission `warehouse.create` | Entrepôt actif |
| Modifier un entrepôt | Mise à jour paramètres | Permission `warehouse.update` | Entrepôt modifié |
| Désactiver un entrepôt | Fermeture temporaire | Permission `warehouse.update` | Entrepôt inactif |
| Supprimer un entrepôt | Suppression définitive | Permission `warehouse.delete` + stock = 0 | Entrepôt supprimé |
| Créer un emplacement | Nouveau point de stockage | Permission `location.create` | Emplacement créé |
| Modifier un emplacement | Mise à jour adressage | Permission `location.update` | Emplacement modifié |
| Supprimer un emplacement | Retrait de l'adressage | Permission `location.delete` + vide | Emplacement supprimé |
| Créer une zone | Définition zone logique | Permission `zone.create` | Zone créée |
| Configurer accès par rôle | Restriction par entrepôt | Permission `warehouse.access` | Droits configurés |
| Imprimer étiquettes | Génération étiquettes emplacement | Permission `location.print` | Étiquettes générées |

### 3.3 Opérations d'entrée en stock

| Action | Description | Pré-requis | Résultat attendu |
|--------|-------------|------------|------------------|
| Créer une entrée | Initialisation du mouvement | Permission `stockEntry.create` | Entrée en brouillon |
| Modifier une entrée | Ajustement avant validation | Permission `stockEntry.update` + non validée | Entrée modifiée |
| Valider une entrée | Confirmation du mouvement | Permission `stockEntry.validate` | Stock mis à jour |
| Annuler une entrée | Annulation avant validation | Permission `stockEntry.cancel` | Entrée annulée |
| Supprimer une entrée | Suppression définitive | Permission `stockEntry.delete` + brouillon | Entrée supprimée |
| Imprimer bordereau | Génération document | Permission `stockEntry.print` | PDF généré |
| Scanner produits | Saisie par code-barres | Permission `stockEntry.create` | Lignes ajoutées |
| Affecter emplacement | Choix du lieu de stockage | Permission `stockEntry.create` | Emplacement défini |
| Saisir lot/série | Renseigner traçabilité | Permission `stockEntry.create` | Traçabilité enregistrée |
| Contre-valider | Annulation après validation | Permission `stockEntry.reverse` | Mouvement inversé |

### 3.4 Opérations de sortie de stock

| Action | Description | Pré-requis | Résultat attendu |
|--------|-------------|------------|------------------|
| Créer une sortie | Initialisation du mouvement | Permission `stockExit.create` | Sortie en brouillon |
| Modifier une sortie | Ajustement avant validation | Permission `stockExit.update` + non validée | Sortie modifiée |
| Valider une sortie | Confirmation du mouvement | Permission `stockExit.validate` + stock dispo | Stock mis à jour |
| Annuler une sortie | Annulation avant validation | Permission `stockExit.cancel` | Sortie annulée |
| Supprimer une sortie | Suppression définitive | Permission `stockExit.delete` + brouillon | Sortie supprimée |
| Imprimer bordereau | Génération document | Permission `stockExit.print` | PDF généré |
| Forcer sortie | Sortie malgré stock insuffisant | Permission `stockExit.force` | Stock négatif autorisé |
| Contre-valider | Annulation après validation | Permission `stockExit.reverse` | Mouvement inversé |

### 3.5 Transferts

| Action | Description | Pré-requis | Résultat attendu |
|--------|-------------|------------|------------------|
| Créer un transfert | Initialisation mouvement | Permission `transfer.create` | Transfert brouillon |
| Modifier un transfert | Ajustement avant validation | Permission `transfer.update` | Transfert modifié |
| Valider transfert | Confirmation départ | Permission `transfer.validate` | Stock en transit |
| Réceptionner transfert | Confirmation arrivée | Permission `transfer.receive` | Stock transféré |
| Annuler transfert | Annulation en cours | Permission `transfer.cancel` | Transfert annulé |
| Scanner emplacements | Saisie origine/destination | Permission `transfer.create` | Emplacements définis |

### 3.6 Ajustements

| Action | Description | Pré-requis | Résultat attendu |
|--------|-------------|------------|------------------|
| Créer un ajustement | Initialisation correction | Permission `adjustment.create` | Ajustement brouillon |
| Saisir quantités | Renseigner écarts | Permission `adjustment.create` | Quantités définies |
| Justifier l'écart | Motif de l'ajustement | Permission `adjustment.create` | Motif enregistré |
| Valider ajustement | Application de la correction | Permission `adjustment.validate` | Stock corrigé |
| Annuler ajustement | Annulation avant validation | Permission `adjustment.cancel` | Ajustement annulé |
| Supprimer ajustement | Suppression définitive | Permission `adjustment.delete` | Ajustement supprimé |

### 3.7 Inventaires

| Action | Description | Pré-requis | Résultat attendu |
|--------|-------------|------------|------------------|
| Créer un inventaire | Initialisation session | Permission `inventory.create` | Inventaire ouvert |
| Définir périmètre | Sélection articles/zones | Permission `inventory.create` | Périmètre défini |
| Geler le stock | Bloquer mouvements | Permission `inventory.freeze` | Stock gelé |
| Ajouter ligne comptage | Saisie article à compter | Permission `inventory.count` | Ligne ajoutée |
| Modifier comptage | Correction quantité | Permission `inventory.count` | Comptage modifié |
| Valider comptage | Confirmation d'une ligne | Permission `inventory.count` | Ligne validée |
| Double comptage | Vérification par 2ème opérateur | Permission `inventory.recount` | Recomptage fait |
| Analyser écarts | Visualiser différences | Permission `inventory.view` | Écarts affichés |
| Valider écarts | Accepter les différences | Permission `inventory.validate` | Écarts acceptés |
| Appliquer ajustements | Régularisation automatique | Permission `inventory.adjust` | Stock mis à jour |
| Clôturer inventaire | Fermeture de la session | Permission `inventory.close` | Inventaire terminé |
| Annuler inventaire | Abandon de la session | Permission `inventory.cancel` | Inventaire annulé |
| Imprimer feuilles | Génération documents comptage | Permission `inventory.print` | Documents générés |

### 3.8 Traçabilité & Audit

| Action | Description | Pré-requis | Résultat attendu |
|--------|-------------|------------|------------------|
| Consulter fiche lot | Visualiser détails lot | Permission `lot.view` | Fiche affichée |
| Créer un lot | Définir nouveau lot | Permission `lot.create` | Lot créé |
| Modifier un lot | Mettre à jour informations | Permission `lot.update` | Lot modifié |
| Bloquer un lot | Mise en quarantaine | Permission `lot.block` | Lot bloqué |
| Consulter fiche série | Visualiser détails série | Permission `serial.view` | Fiche affichée |
| Rechercher historique | Filtrer mouvements | Permission `history.view` | Résultats affichés |
| Exporter historique | Télécharger données | Permission `history.export` | Fichier généré |
| Auditer utilisateur | Voir actions d'un user | Permission `audit.view` | Journal affiché |
| Tracer lot ascendant | Remonter à l'origine | Permission `trace.view` | Chaîne affichée |
| Tracer lot descendant | Suivre la distribution | Permission `trace.view` | Chaîne affichée |

### 3.9 Reporting & Analyses

| Action | Description | Pré-requis | Résultat attendu |
|--------|-------------|------------|------------------|
| Consulter état stocks | Visualiser quantités | Permission `report.stock` | Rapport affiché |
| Consulter valorisation | Voir valeur du stock | Permission `report.valuation` | Valorisation affichée |
| Consulter écarts | Rapport d'inventaire | Permission `report.variance` | Écarts affichés |
| Consulter rotations | Indicateurs performance | Permission `report.kpi` | KPIs affichés |
| Configurer alertes | Paramétrer notifications | Permission `alert.config` | Alertes configurées |
| Recevoir alertes | Notification seuils | Permission `alert.receive` | Alertes reçues |
| Exporter PDF | Télécharger en PDF | Permission `report.export` | PDF généré |
| Exporter Excel | Télécharger en Excel | Permission `report.export` | Excel généré |
| Programmer rapport | Planifier envoi auto | Permission `report.schedule` | Planification active |
| Créer tableau de bord | Dashboard personnalisé | Permission `dashboard.create` | Dashboard créé |

---

## 4. MATRICE DES PERMISSIONS

### 4.1 Permissions - Référentiel

| Code Permission | Libellé | Module | Type d'accès | Description |
|-----------------|---------|--------|--------------|-------------|
| `article.view` | Voir articles | Articles | Lecture | Consulter la liste et fiches articles |
| `article.create` | Créer article | Articles | Écriture | Créer une nouvelle fiche article |
| `article.update` | Modifier article | Articles | Écriture | Modifier les informations d'un article |
| `article.archive` | Archiver article | Articles | Écriture | Désactiver un article sans le supprimer |
| `article.delete` | Supprimer article | Articles | Suppression | Supprimer définitivement un article |
| `article.pricing` | Gérer prix | Articles | Écriture | Configurer prix, remises et taxes |
| `article.import` | Importer articles | Articles | Écriture | Import en masse via fichier |
| `article.export` | Exporter articles | Articles | Lecture | Export de la liste articles |
| `category.view` | Voir catégories | Catégories | Lecture | Consulter l'arborescence |
| `category.manage` | Gérer catégories | Catégories | Écriture | Créer/modifier/supprimer catégories |
| `uom.view` | Voir unités | Unités | Lecture | Consulter les unités de mesure |
| `uom.manage` | Gérer unités | Unités | Écriture | Créer/modifier/supprimer unités |

### 4.2 Permissions - Infrastructure

| Code Permission | Libellé | Module | Type d'accès | Description |
|-----------------|---------|--------|--------------|-------------|
| `warehouse.view` | Voir entrepôts | Entrepôts | Lecture | Consulter la liste des entrepôts |
| `warehouse.create` | Créer entrepôt | Entrepôts | Écriture | Créer un nouvel entrepôt |
| `warehouse.update` | Modifier entrepôt | Entrepôts | Écriture | Modifier les paramètres |
| `warehouse.delete` | Supprimer entrepôt | Entrepôts | Suppression | Supprimer un entrepôt |
| `warehouse.access` | Gérer accès entrepôt | Entrepôts | Administration | Configurer les droits par entrepôt |
| `zone.view` | Voir zones | Zones | Lecture | Consulter les zones |
| `zone.create` | Créer zone | Zones | Écriture | Créer une nouvelle zone |
| `zone.update` | Modifier zone | Zones | Écriture | Modifier une zone |
| `zone.delete` | Supprimer zone | Zones | Suppression | Supprimer une zone |
| `location.view` | Voir emplacements | Emplacements | Lecture | Consulter les emplacements |
| `location.create` | Créer emplacement | Emplacements | Écriture | Créer un nouvel emplacement |
| `location.update` | Modifier emplacement | Emplacements | Écriture | Modifier un emplacement |
| `location.delete` | Supprimer emplacement | Emplacements | Suppression | Supprimer un emplacement |
| `location.print` | Imprimer étiquettes | Emplacements | Lecture | Générer étiquettes emplacements |

### 4.3 Permissions - Opérations d'entrée

| Code Permission | Libellé | Module | Type d'accès | Description |
|-----------------|---------|--------|--------------|-------------|
| `stockEntry.view` | Voir entrées | Entrées | Lecture | Consulter les entrées en stock |
| `stockEntry.create` | Créer entrée | Entrées | Écriture | Créer une nouvelle entrée |
| `stockEntry.update` | Modifier entrée | Entrées | Écriture | Modifier une entrée non validée |
| `stockEntry.validate` | Valider entrée | Entrées | Validation | Confirmer une entrée en stock |
| `stockEntry.cancel` | Annuler entrée | Entrées | Écriture | Annuler une entrée non validée |
| `stockEntry.delete` | Supprimer entrée | Entrées | Suppression | Supprimer une entrée brouillon |
| `stockEntry.print` | Imprimer bordereau | Entrées | Lecture | Générer document d'entrée |
| `stockEntry.reverse` | Contre-passer entrée | Entrées | Validation | Annuler une entrée validée |

### 4.4 Permissions - Opérations de sortie

| Code Permission | Libellé | Module | Type d'accès | Description |
|-----------------|---------|--------|--------------|-------------|
| `stockExit.view` | Voir sorties | Sorties | Lecture | Consulter les sorties de stock |
| `stockExit.create` | Créer sortie | Sorties | Écriture | Créer une nouvelle sortie |
| `stockExit.update` | Modifier sortie | Sorties | Écriture | Modifier une sortie non validée |
| `stockExit.validate` | Valider sortie | Sorties | Validation | Confirmer une sortie de stock |
| `stockExit.cancel` | Annuler sortie | Sorties | Écriture | Annuler une sortie non validée |
| `stockExit.delete` | Supprimer sortie | Sorties | Suppression | Supprimer une sortie brouillon |
| `stockExit.print` | Imprimer bordereau | Sorties | Lecture | Générer document de sortie |
| `stockExit.force` | Forcer sortie | Sorties | Administration | Sortie malgré stock insuffisant |
| `stockExit.reverse` | Contre-passer sortie | Sorties | Validation | Annuler une sortie validée |

### 4.5 Permissions - Transferts

| Code Permission | Libellé | Module | Type d'accès | Description |
|-----------------|---------|--------|--------------|-------------|
| `transfer.view` | Voir transferts | Transferts | Lecture | Consulter les transferts |
| `transfer.create` | Créer transfert | Transferts | Écriture | Initier un nouveau transfert |
| `transfer.update` | Modifier transfert | Transferts | Écriture | Modifier un transfert non validé |
| `transfer.validate` | Valider transfert | Transferts | Validation | Confirmer le départ |
| `transfer.receive` | Réceptionner transfert | Transferts | Validation | Confirmer l'arrivée |
| `transfer.cancel` | Annuler transfert | Transferts | Écriture | Annuler un transfert en cours |

### 4.6 Permissions - Ajustements

| Code Permission | Libellé | Module | Type d'accès | Description |
|-----------------|---------|--------|--------------|-------------|
| `adjustment.view` | Voir ajustements | Ajustements | Lecture | Consulter les ajustements |
| `adjustment.create` | Créer ajustement | Ajustements | Écriture | Créer un nouvel ajustement |
| `adjustment.update` | Modifier ajustement | Ajustements | Écriture | Modifier un ajustement non validé |
| `adjustment.validate` | Valider ajustement | Ajustements | Validation | Confirmer l'ajustement |
| `adjustment.cancel` | Annuler ajustement | Ajustements | Écriture | Annuler un ajustement |
| `adjustment.delete` | Supprimer ajustement | Ajustements | Suppression | Supprimer un ajustement brouillon |

### 4.7 Permissions - Inventaires

| Code Permission | Libellé | Module | Type d'accès | Description |
|-----------------|---------|--------|--------------|-------------|
| `inventory.view` | Voir inventaires | Inventaire | Lecture | Consulter les sessions d'inventaire |
| `inventory.create` | Créer inventaire | Inventaire | Écriture | Démarrer une nouvelle session |
| `inventory.count` | Saisir comptages | Inventaire | Écriture | Renseigner les quantités comptées |
| `inventory.recount` | Double comptage | Inventaire | Écriture | Effectuer un recomptage |
| `inventory.validate` | Valider écarts | Inventaire | Validation | Accepter les différences |
| `inventory.adjust` | Appliquer ajustements | Inventaire | Validation | Régulariser le stock |
| `inventory.close` | Clôturer inventaire | Inventaire | Validation | Fermer la session |
| `inventory.cancel` | Annuler inventaire | Inventaire | Écriture | Abandonner la session |
| `inventory.freeze` | Geler stock | Inventaire | Administration | Bloquer les mouvements |
| `inventory.print` | Imprimer feuilles | Inventaire | Lecture | Générer documents de comptage |

### 4.8 Permissions - Traçabilité

| Code Permission | Libellé | Module | Type d'accès | Description |
|-----------------|---------|--------|--------------|-------------|
| `lot.view` | Voir lots | Lots | Lecture | Consulter les fiches lots |
| `lot.create` | Créer lot | Lots | Écriture | Définir un nouveau lot |
| `lot.update` | Modifier lot | Lots | Écriture | Mettre à jour un lot |
| `lot.block` | Bloquer lot | Lots | Écriture | Mettre en quarantaine |
| `serial.view` | Voir séries | Séries | Lecture | Consulter les numéros de série |
| `serial.manage` | Gérer séries | Séries | Écriture | Créer/modifier les séries |
| `history.view` | Voir historique | Historique | Lecture | Consulter les mouvements |
| `history.export` | Exporter historique | Historique | Lecture | Télécharger l'historique |
| `trace.view` | Voir traçabilité | Traçabilité | Lecture | Accéder aux chaînes de traçabilité |
| `audit.view` | Voir audit | Audit | Lecture | Consulter le journal d'audit |
| `audit.export` | Exporter audit | Audit | Lecture | Télécharger le journal |

### 4.9 Permissions - Reporting

| Code Permission | Libellé | Module | Type d'accès | Description |
|-----------------|---------|--------|--------------|-------------|
| `report.stock` | Rapport état stocks | Reporting | Lecture | Voir les quantités en stock |
| `report.valuation` | Rapport valorisation | Reporting | Lecture | Voir la valeur du stock |
| `report.variance` | Rapport écarts | Reporting | Lecture | Voir les écarts d'inventaire |
| `report.kpi` | Rapport KPIs | Reporting | Lecture | Voir les indicateurs |
| `report.export` | Exporter rapports | Reporting | Lecture | Télécharger en PDF/Excel |
| `report.schedule` | Programmer rapports | Reporting | Écriture | Planifier envois automatiques |
| `dashboard.view` | Voir tableaux de bord | Dashboard | Lecture | Consulter les dashboards |
| `dashboard.create` | Créer tableau de bord | Dashboard | Écriture | Définir un dashboard perso |
| `alert.config` | Configurer alertes | Alertes | Écriture | Paramétrer les notifications |
| `alert.receive` | Recevoir alertes | Alertes | Lecture | Être notifié des seuils |

### 4.10 Permissions - Administration

| Code Permission | Libellé | Module | Type d'accès | Description |
|-----------------|---------|--------|--------------|-------------|
| `settings.stock` | Paramètres stock | Administration | Administration | Configurer le module |
| `settings.valuation` | Paramètres valorisation | Administration | Administration | Configurer méthodes de valorisation |
| `settings.workflow` | Paramètres workflow | Administration | Administration | Configurer les flux de validation |
| `role.view` | Voir rôles | Permissions | Lecture | Consulter les rôles |
| `role.manage` | Gérer rôles | Permissions | Administration | Créer/modifier les rôles |
| `permission.assign` | Attribuer permissions | Permissions | Administration | Assigner les droits |
| `user.warehouse.assign` | Affecter entrepôts | Permissions | Administration | Lier utilisateurs et entrepôts |

---

## 5. RÔLES TYPES ET ATTRIBUTIONS

### 5.1 Définition des rôles

| Rôle | Code | Description | Niveau |
|------|------|-------------|--------|
| **Super Administrateur** | `SUPER_ADMIN` | Accès complet à toutes les fonctionnalités | 100 |
| **Administrateur Stock** | `STOCK_ADMIN` | Gestion complète du module stock | 90 |
| **Responsable Entrepôt** | `WAREHOUSE_MANAGER` | Supervision d'un ou plusieurs entrepôts | 80 |
| **Chef d'équipe** | `TEAM_LEADER` | Gestion opérationnelle d'une équipe | 70 |
| **Magasinier Senior** | `SENIOR_OPERATOR` | Opérations avec validation | 60 |
| **Magasinier** | `OPERATOR` | Opérations courantes | 50 |
| **Inventoriste** | `INVENTORY_CLERK` | Spécialisé inventaires | 50 |
| **Contrôleur Qualité** | `QUALITY_CONTROLLER` | Contrôle et blocage lots | 60 |
| **Analyste** | `ANALYST` | Consultation et reporting | 40 |
| **Consultant** | `VIEWER` | Lecture seule | 10 |

---

### 5.2 Matrice Rôles / Permissions

#### Légende
- ✅ = Permission accordée
- ❌ = Permission refusée
- 🔶 = Restreint (selon périmètre)

#### A. Référentiel

| Permission | Super Admin | Stock Admin | Warehouse Mgr | Team Leader | Senior Op. | Operator | Inventory | Quality | Analyst | Viewer |
|------------|:-----------:|:-----------:|:-------------:|:-----------:|:----------:|:--------:|:---------:|:-------:|:-------:|:------:|
| `article.view` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `article.create` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `article.update` | ✅ | ✅ | ✅ | 🔶 | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `article.archive` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `article.delete` | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `article.pricing` | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `article.import` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `article.export` | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ |
| `category.view` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `category.manage` | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `uom.view` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `uom.manage` | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |

#### B. Infrastructure

| Permission | Super Admin | Stock Admin | Warehouse Mgr | Team Leader | Senior Op. | Operator | Inventory | Quality | Analyst | Viewer |
|------------|:-----------:|:-----------:|:-------------:|:-----------:|:----------:|:--------:|:---------:|:-------:|:-------:|:------:|
| `warehouse.view` | ✅ | ✅ | 🔶 | 🔶 | 🔶 | 🔶 | 🔶 | 🔶 | ✅ | 🔶 |
| `warehouse.create` | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `warehouse.update` | ✅ | ✅ | 🔶 | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `warehouse.delete` | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `warehouse.access` | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `zone.view` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `zone.create` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `zone.update` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `zone.delete` | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `location.view` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `location.create` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `location.update` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `location.delete` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `location.print` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |

#### C. Opérations d'entrée

| Permission | Super Admin | Stock Admin | Warehouse Mgr | Team Leader | Senior Op. | Operator | Inventory | Quality | Analyst | Viewer |
|------------|:-----------:|:-----------:|:-------------:|:-----------:|:----------:|:--------:|:---------:|:-------:|:-------:|:------:|
| `stockEntry.view` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `stockEntry.create` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| `stockEntry.update` | ✅ | ✅ | ✅ | ✅ | ✅ | 🔶 | ❌ | ❌ | ❌ | ❌ |
| `stockEntry.validate` | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `stockEntry.cancel` | ✅ | ✅ | ✅ | ✅ | ✅ | 🔶 | ❌ | ❌ | ❌ | ❌ |
| `stockEntry.delete` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `stockEntry.print` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| `stockEntry.reverse` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |

#### D. Opérations de sortie

| Permission | Super Admin | Stock Admin | Warehouse Mgr | Team Leader | Senior Op. | Operator | Inventory | Quality | Analyst | Viewer |
|------------|:-----------:|:-----------:|:-------------:|:-----------:|:----------:|:--------:|:---------:|:-------:|:-------:|:------:|
| `stockExit.view` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `stockExit.create` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| `stockExit.update` | ✅ | ✅ | ✅ | ✅ | ✅ | 🔶 | ❌ | ❌ | ❌ | ❌ |
| `stockExit.validate` | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `stockExit.cancel` | ✅ | ✅ | ✅ | ✅ | ✅ | 🔶 | ❌ | ❌ | ❌ | ❌ |
| `stockExit.delete` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `stockExit.print` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| `stockExit.force` | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `stockExit.reverse` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |

#### E. Transferts

| Permission | Super Admin | Stock Admin | Warehouse Mgr | Team Leader | Senior Op. | Operator | Inventory | Quality | Analyst | Viewer |
|------------|:-----------:|:-----------:|:-------------:|:-----------:|:----------:|:--------:|:---------:|:-------:|:-------:|:------:|
| `transfer.view` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | ✅ |
| `transfer.create` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| `transfer.update` | ✅ | ✅ | ✅ | ✅ | ✅ | 🔶 | ❌ | ❌ | ❌ | ❌ |
| `transfer.validate` | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `transfer.receive` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| `transfer.cancel` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |

#### F. Ajustements

| Permission | Super Admin | Stock Admin | Warehouse Mgr | Team Leader | Senior Op. | Operator | Inventory | Quality | Analyst | Viewer |
|------------|:-----------:|:-----------:|:-------------:|:-----------:|:----------:|:--------:|:---------:|:-------:|:-------:|:------:|
| `adjustment.view` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `adjustment.create` | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ |
| `adjustment.update` | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ |
| `adjustment.validate` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `adjustment.cancel` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `adjustment.delete` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |

#### G. Inventaires

| Permission | Super Admin | Stock Admin | Warehouse Mgr | Team Leader | Senior Op. | Operator | Inventory | Quality | Analyst | Viewer |
|------------|:-----------:|:-----------:|:-------------:|:-----------:|:----------:|:--------:|:---------:|:-------:|:-------:|:------:|
| `inventory.view` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `inventory.create` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ |
| `inventory.count` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| `inventory.recount` | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ |
| `inventory.validate` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `inventory.adjust` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `inventory.close` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `inventory.cancel` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `inventory.freeze` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `inventory.print` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |

#### H. Traçabilité

| Permission | Super Admin | Stock Admin | Warehouse Mgr | Team Leader | Senior Op. | Operator | Inventory | Quality | Analyst | Viewer |
|------------|:-----------:|:-----------:|:-------------:|:-----------:|:----------:|:--------:|:---------:|:-------:|:-------:|:------:|
| `lot.view` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `lot.create` | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ |
| `lot.update` | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ |
| `lot.block` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ |
| `serial.view` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `serial.manage` | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `history.view` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `history.export` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ✅ | ✅ | ❌ |
| `trace.view` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `audit.view` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | ❌ |
| `audit.export` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |

#### I. Reporting

| Permission | Super Admin | Stock Admin | Warehouse Mgr | Team Leader | Senior Op. | Operator | Inventory | Quality | Analyst | Viewer |
|------------|:-----------:|:-----------:|:-------------:|:-----------:|:----------:|:--------:|:---------:|:-------:|:-------:|:------:|
| `report.stock` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `report.valuation` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |
| `report.variance` | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ❌ |
| `report.kpi` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |
| `report.export` | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ❌ |
| `report.schedule` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |
| `dashboard.view` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `dashboard.create` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |
| `alert.config` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `alert.receive` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |

#### J. Administration

| Permission | Super Admin | Stock Admin | Warehouse Mgr | Team Leader | Senior Op. | Operator | Inventory | Quality | Analyst | Viewer |
|------------|:-----------:|:-----------:|:-------------:|:-----------:|:----------:|:--------:|:---------:|:-------:|:-------:|:------:|
| `settings.stock` | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `settings.valuation` | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `settings.workflow` | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `role.view` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `role.manage` | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `permission.assign` | ✅ | ✅ | 🔶 | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `user.warehouse.assign` | ✅ | ✅ | 🔶 | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |

---

## 6. ANNEXES

### 6.1 Glossaire

| Terme | Définition |
|-------|------------|
| **FIFO** | First In First Out - Méthode de valorisation où les premiers articles entrés sont les premiers sortis |
| **LIFO** | Last In First Out - Méthode où les derniers articles entrés sont les premiers sortis |
| **CUMP** | Coût Unitaire Moyen Pondéré - Moyenne des coûts d'acquisition |
| **UoM** | Unit of Measure - Unité de mesure |
| **SKU** | Stock Keeping Unit - Référence unique d'un article |
| **DLC** | Date Limite de Consommation |
| **DLUO** | Date Limite d'Utilisation Optimale |
| **Picking** | Zone de prélèvement pour préparation de commandes |
| **Quarantaine** | Zone de blocage pour contrôle qualité |

### 6.2 Règles de gestion clés

1. **Principe de séparation des tâches**
   - L'opérateur qui saisit ne peut pas valider ses propres opérations
   - Double validation requise pour les ajustements > seuil paramétrable

2. **Traçabilité obligatoire**
   - Tout mouvement doit être justifié et horodaté
   - Conservation de l'historique pendant durée légale (10 ans min.)

3. **Contrôle des stocks négatifs**
   - Stock négatif interdit par défaut
   - Dérogation uniquement avec permission `stockExit.force`

4. **Gel inventaire**
   - Blocage automatique des mouvements pendant comptage
   - Déblocage uniquement après clôture ou annulation

### 6.3 Workflows de validation suggérés

```
┌─────────────────────────────────────────────────────────────────┐
│                    WORKFLOW ENTRÉE EN STOCK                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Opérateur          Team Leader         Warehouse Manager        │
│      │                   │                      │                │
│      ▼                   │                      │                │
│  [Création]              │                      │                │
│      │                   │                      │                │
│      ▼                   │                      │                │
│  [Saisie lignes]         │                      │                │
│      │                   │                      │                │
│      └──────────────────►│                      │                │
│                          ▼                      │                │
│                    [Validation]                 │                │
│                          │                      │                │
│                          │ Si montant > seuil   │                │
│                          └─────────────────────►│                │
│                                                 ▼                │
│                                           [Approbation]          │
│                                                 │                │
│                                                 ▼                │
│                                          [Stock mis à jour]      │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────────┐
│                    WORKFLOW INVENTAIRE                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Inventory Clerk      Team Leader      Warehouse Mgr   Stock Adm │
│        │                  │                 │              │     │
│        ▼                  │                 │              │     │
│   [Comptage]              │                 │              │     │
│        │                  │                 │              │     │
│        └─────────────────►│                 │              │     │
│                           ▼                 │              │     │
│                    [Recomptage]             │              │     │
│                           │                 │              │     │
│                           └────────────────►│              │     │
│                                             ▼              │     │
│                                      [Validation           │     │
│                                        écarts]             │     │
│                                             │              │     │
│                                             │ Si > seuil   │     │
│                                             └─────────────►│     │
│                                                            ▼     │
│                                                     [Approbation]│
│                                                            │     │
│                                                            ▼     │
│                                                   [Ajustement    │
│                                                     automatique] │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### 6.4 Codes d'erreur standards

| Code | Message | Action corrective |
|------|---------|-------------------|
| `STK-001` | Stock insuffisant | Vérifier disponibilité ou demander dérogation |
| `STK-002` | Article inactif | Réactiver l'article ou choisir un autre |
| `STK-003` | Lot bloqué | Contacter le contrôle qualité |
| `STK-004` | Emplacement invalide | Vérifier l'adressage |
| `STK-005` | Mouvement en cours | Attendre la fin du traitement |
| `STK-006` | Inventaire en cours | Attendre clôture de l'inventaire |
| `STK-007` | Permission refusée | Contacter l'administrateur |
| `STK-008` | Donnée obligatoire manquante | Compléter les champs requis |
| `STK-009` | Lot/Série déjà existant | Utiliser une référence unique |
| `STK-010` | Date de péremption dépassée | Bloquer le lot ou effectuer une sortie |

---

## 📝 Historique des versions

| Version | Date | Auteur | Modifications |
|---------|------|--------|---------------|
| 1.0 | - | - | Version initiale |
| 2.0 | Janvier 2026 | - | Refonte complète : ajout rôles, permissions étendues, workflows, annexes |

---

> **Document généré pour intégration ERP**  
> Pour toute question : consulter la documentation technique associée
