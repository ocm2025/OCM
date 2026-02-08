# CLAUDE.md - Skill Développement Module Gestion de Stock

> **Objectif :** Guider Claude Code dans le développement complet d'un module de gestion de stock pour ERP
> **Version :** 1.0
> **Stack technique recommandée :** React/Next.js + FastAPI/Node.js + MongoDB/PostgreSQL

---

## 📋 TABLE DES MATIÈRES

1. [Vue d'ensemble du projet](#1-vue-densemble-du-projet)
2. [Architecture technique](#2-architecture-technique)
3. [Modèles de données](#3-modèles-de-données)
4. [API Endpoints](#4-api-endpoints)
5. [Système de permissions](#5-système-de-permissions)
6. [Règles métier](#6-règles-métier)
7. [Composants UI](#7-composants-ui)
8. [Workflows & États](#8-workflows--états)
9. [Tests requis](#9-tests-requis)
10. [Checklist de développement](#10-checklist-de-développement)

---

## 1. VUE D'ENSEMBLE DU PROJET

### 1.1 Description

Module de gestion de stock complet comprenant :
- Gestion du référentiel (articles, catégories, entrepôts, emplacements)
- Opérations de stock (entrées, sorties, transferts, ajustements)
- Inventaires (tournant, annuel, partiel)
- Traçabilité (lots, séries, historique)
- Reporting et analyses

### 1.2 Modules à développer (ordre de priorité)

```
Phase 1 - Fondations (Sprint 1-2)
├── 1.1 Authentification & Permissions
├── 1.2 Gestion des articles
├── 1.3 Gestion des entrepôts & emplacements
└── 1.4 Unités de mesure & catégories

Phase 2 - Opérations (Sprint 3-4)
├── 2.1 Entrées en stock
├── 2.2 Sorties de stock
├── 2.3 Transferts
└── 2.4 Ajustements

Phase 3 - Inventaires (Sprint 5)
├── 3.1 Création & paramétrage inventaire
├── 3.2 Comptage & saisie
└── 3.3 Validation & ajustement automatique

Phase 4 - Traçabilité (Sprint 6)
├── 4.1 Gestion des lots
├── 4.2 Gestion des numéros de série
└── 4.3 Historique & audit

Phase 5 - Reporting (Sprint 7)
├── 5.1 État des stocks
├── 5.2 Valorisation
├── 5.3 KPIs & dashboards
└── 5.4 Alertes & notifications
```

---

## 2. ARCHITECTURE TECHNIQUE

### 2.1 Structure des dossiers

```
/app
├── /backend
│   ├── /api
│   │   ├── /routes
│   │   │   ├── auth.py
│   │   │   ├── articles.py
│   │   │   ├── warehouses.py
│   │   │   ├── locations.py
│   │   │   ├── stock_entries.py
│   │   │   ├── stock_exits.py
│   │   │   ├── transfers.py
│   │   │   ├── adjustments.py
│   │   │   ├── inventories.py
│   │   │   ├── lots.py
│   │   │   ├── reports.py
│   │   │   └── settings.py
│   │   └── __init__.py
│   ├── /models
│   │   ├── user.py
│   │   ├── article.py
│   │   ├── category.py
│   │   ├── warehouse.py
│   │   ├── location.py
│   │   ├── stock_movement.py
│   │   ├── inventory.py
│   │   ├── lot.py
│   │   └── audit_log.py
│   ├── /services
│   │   ├── stock_service.py
│   │   ├── inventory_service.py
│   │   ├── valuation_service.py
│   │   └── notification_service.py
│   ├── /middleware
│   │   ├── auth.py
│   │   └── permissions.py
│   ├── /utils
│   │   ├── validators.py
│   │   ├── calculators.py
│   │   └── formatters.py
│   ├── server.py
│   └── requirements.txt
│
├── /frontend
│   ├── /src
│   │   ├── /components
│   │   │   ├── /ui (composants génériques)
│   │   │   ├── /articles
│   │   │   ├── /warehouses
│   │   │   ├── /stock-operations
│   │   │   ├── /inventory
│   │   │   ├── /traceability
│   │   │   └── /reports
│   │   ├── /pages
│   │   ├── /hooks
│   │   ├── /services
│   │   ├── /store (état global)
│   │   ├── /utils
│   │   └── /types
│   └── package.json
│
└── /docs
    ├── API.md
    ├── PERMISSIONS.md
    └── WORKFLOWS.md
```

### 2.2 Conventions de nommage

| Élément | Convention | Exemple |
|---------|------------|---------|
| Tables/Collections | snake_case pluriel | `stock_movements` |
| Modèles Python | PascalCase singulier | `StockMovement` |
| Endpoints API | kebab-case pluriel | `/api/stock-entries` |
| Composants React | PascalCase | `StockEntryForm.jsx` |
| Fichiers JS/TS | kebab-case ou camelCase | `stock-entry-form.jsx` |
| Variables | camelCase | `totalQuantity` |
| Constantes | SCREAMING_SNAKE_CASE | `MAX_STOCK_QUANTITY` |
| Permissions | dot.notation | `stockEntry.create` |

---

## 3. MODÈLES DE DONNÉES

### 3.1 Article (Product)

```python
class Article:
    _id: ObjectId
    code: str                    # SKU unique
    name: str                    # Libellé
    description: str             # Description longue
    category_id: ObjectId        # Référence catégorie
    unit_of_measure_id: ObjectId # Unité principale
    secondary_uom_id: ObjectId   # Unité secondaire (optionnel)
    conversion_factor: float     # Facteur de conversion
    
    # Paramètres stock
    min_stock: float             # Stock minimum
    max_stock: float             # Stock maximum
    security_stock: float        # Stock de sécurité
    reorder_point: float         # Point de réapprovisionnement
    reorder_quantity: float      # Quantité à commander
    
    # Traçabilité
    tracking_type: str           # 'none' | 'lot' | 'serial'
    has_expiry_date: bool        # Gestion DLC/DLUO
    
    # Prix
    unit_cost: float             # Coût unitaire
    sale_price: float            # Prix de vente
    tax_rate: float              # Taux de TVA
    
    # Médias
    images: List[str]            # URLs des images
    barcode: str                 # Code-barres EAN
    qr_code: str                 # QR Code
    
    # Métadonnées
    is_active: bool
    created_at: datetime
    updated_at: datetime
    created_by: ObjectId
```

### 3.2 Entrepôt (Warehouse)

```python
class Warehouse:
    _id: ObjectId
    code: str                    # Code unique
    name: str                    # Nom
    address: dict                # Adresse complète
    type: str                    # 'main' | 'secondary' | 'transit'
    is_active: bool
    
    # Paramètres
    allow_negative_stock: bool   # Autoriser stock négatif
    default_valuation_method: str # 'fifo' | 'lifo' | 'average'
    
    created_at: datetime
    updated_at: datetime
```

### 3.3 Emplacement (Location)

```python
class Location:
    _id: ObjectId
    warehouse_id: ObjectId       # Référence entrepôt
    code: str                    # Adresse (ex: A-01-02-03)
    name: str                    # Libellé
    zone: str                    # 'reception' | 'storage' | 'picking' | 'shipping' | 'quarantine'
    
    # Hiérarchie (optionnel)
    aisle: str                   # Allée
    rack: str                    # Rack
    level: str                   # Niveau
    position: str                # Position
    
    # Capacité
    max_weight: float            # Poids max (kg)
    max_volume: float            # Volume max (m³)
    
    is_active: bool
    created_at: datetime
```

### 3.4 Mouvement de Stock (StockMovement)

```python
class StockMovement:
    _id: ObjectId
    reference: str               # Numéro unique auto-généré
    type: str                    # 'entry' | 'exit' | 'transfer' | 'adjustment'
    subtype: str                 # 'purchase' | 'return' | 'production' | 'sale' | 'consumption' | 'loss'
    
    status: str                  # 'draft' | 'pending' | 'validated' | 'cancelled' | 'reversed'
    
    # Localisation
    source_warehouse_id: ObjectId
    source_location_id: ObjectId
    dest_warehouse_id: ObjectId
    dest_location_id: ObjectId
    
    # Lignes
    lines: List[StockMovementLine]
    
    # Valorisation
    total_value: float
    currency: str
    
    # Dates
    planned_date: datetime       # Date prévue
    effective_date: datetime     # Date effective
    
    # Documents liés
    related_document_type: str   # 'purchase_order' | 'sale_order' | 'inventory'
    related_document_id: ObjectId
    
    # Audit
    notes: str
    created_at: datetime
    created_by: ObjectId
    validated_at: datetime
    validated_by: ObjectId
    
class StockMovementLine:
    article_id: ObjectId
    quantity: float
    unit_of_measure_id: ObjectId
    unit_cost: float
    total_cost: float
    lot_id: ObjectId             # Si traçabilité lot
    serial_number: str           # Si traçabilité série
    expiry_date: datetime        # Si gestion DLC
```

### 3.5 Inventaire (Inventory)

```python
class Inventory:
    _id: ObjectId
    reference: str               # Numéro unique
    name: str                    # Libellé
    type: str                    # 'annual' | 'rotating' | 'partial'
    status: str                  # 'draft' | 'in_progress' | 'counting' | 'validation' | 'closed' | 'cancelled'
    
    # Périmètre
    warehouse_id: ObjectId
    location_ids: List[ObjectId] # Emplacements concernés (si partiel)
    category_ids: List[ObjectId] # Catégories concernées (si partiel)
    article_ids: List[ObjectId]  # Articles concernés (si partiel)
    
    # Dates
    planned_date: datetime
    start_date: datetime
    end_date: datetime
    
    # Comptages
    lines: List[InventoryLine]
    
    # Résultats
    total_theoretical_value: float
    total_counted_value: float
    total_variance_value: float
    
    # Audit
    created_at: datetime
    created_by: ObjectId
    closed_at: datetime
    closed_by: ObjectId

class InventoryLine:
    article_id: ObjectId
    location_id: ObjectId
    lot_id: ObjectId
    
    theoretical_quantity: float  # Quantité système
    counted_quantity: float      # Quantité comptée
    variance_quantity: float     # Écart
    variance_percentage: float   # % écart
    
    count_1: float               # Premier comptage
    count_1_by: ObjectId
    count_1_at: datetime
    
    count_2: float               # Second comptage (si requis)
    count_2_by: ObjectId
    count_2_at: datetime
    
    status: str                  # 'pending' | 'counted' | 'validated' | 'adjusted'
    adjustment_reason: str
```

### 3.6 Lot

```python
class Lot:
    _id: ObjectId
    number: str                  # Numéro de lot unique
    article_id: ObjectId
    
    # Traçabilité
    supplier_lot: str            # N° lot fournisseur
    supplier_id: ObjectId
    
    # Dates
    manufacturing_date: datetime
    expiry_date: datetime
    reception_date: datetime
    
    # Quantités
    initial_quantity: float
    current_quantity: float
    
    # Statut
    status: str                  # 'available' | 'reserved' | 'blocked' | 'quarantine' | 'expired'
    block_reason: str
    
    # Audit
    created_at: datetime
    created_by: ObjectId
```

### 3.7 Stock en temps réel (StockLevel)

```python
class StockLevel:
    _id: ObjectId
    article_id: ObjectId
    warehouse_id: ObjectId
    location_id: ObjectId
    lot_id: ObjectId             # Optionnel
    
    # Quantités
    quantity_on_hand: float      # Stock physique
    quantity_reserved: float     # Réservé
    quantity_available: float    # Disponible (on_hand - reserved)
    quantity_incoming: float     # En cours de réception
    quantity_outgoing: float     # En cours d'expédition
    
    # Valorisation
    unit_cost: float
    total_value: float
    
    last_movement_at: datetime
    last_inventory_at: datetime
```

### 3.8 Journal d'audit (AuditLog)

```python
class AuditLog:
    _id: ObjectId
    timestamp: datetime
    user_id: ObjectId
    user_name: str
    
    action: str                  # 'create' | 'update' | 'delete' | 'validate' | 'cancel'
    entity_type: str             # 'article' | 'stock_movement' | 'inventory' | etc.
    entity_id: ObjectId
    entity_reference: str
    
    changes: dict                # {field: {old: x, new: y}}
    ip_address: str
    user_agent: str
```

---

## 4. API ENDPOINTS

### 4.1 Articles

```
GET    /api/articles              # Liste paginée + filtres
GET    /api/articles/:id          # Détail article
POST   /api/articles              # Créer article
PUT    /api/articles/:id          # Modifier article
DELETE /api/articles/:id          # Supprimer article
PATCH  /api/articles/:id/archive  # Archiver article
PATCH  /api/articles/:id/activate # Activer/désactiver
POST   /api/articles/import       # Import CSV/Excel
GET    /api/articles/export       # Export CSV/Excel
GET    /api/articles/:id/stock    # Stock par article
GET    /api/articles/:id/movements # Mouvements de l'article
```

### 4.2 Entrepôts & Emplacements

```
# Entrepôts
GET    /api/warehouses
GET    /api/warehouses/:id
POST   /api/warehouses
PUT    /api/warehouses/:id
DELETE /api/warehouses/:id
GET    /api/warehouses/:id/stock  # Stock de l'entrepôt

# Emplacements
GET    /api/warehouses/:warehouseId/locations
GET    /api/locations/:id
POST   /api/locations
PUT    /api/locations/:id
DELETE /api/locations/:id
GET    /api/locations/:id/stock   # Stock de l'emplacement
```

### 4.3 Opérations de stock

```
# Entrées
GET    /api/stock-entries
GET    /api/stock-entries/:id
POST   /api/stock-entries
PUT    /api/stock-entries/:id
DELETE /api/stock-entries/:id
POST   /api/stock-entries/:id/validate
POST   /api/stock-entries/:id/cancel
POST   /api/stock-entries/:id/reverse

# Sorties
GET    /api/stock-exits
GET    /api/stock-exits/:id
POST   /api/stock-exits
PUT    /api/stock-exits/:id
DELETE /api/stock-exits/:id
POST   /api/stock-exits/:id/validate
POST   /api/stock-exits/:id/cancel
POST   /api/stock-exits/:id/reverse

# Transferts
GET    /api/transfers
GET    /api/transfers/:id
POST   /api/transfers
PUT    /api/transfers/:id
POST   /api/transfers/:id/validate   # Départ
POST   /api/transfers/:id/receive    # Arrivée
POST   /api/transfers/:id/cancel

# Ajustements
GET    /api/adjustments
GET    /api/adjustments/:id
POST   /api/adjustments
PUT    /api/adjustments/:id
DELETE /api/adjustments/:id
POST   /api/adjustments/:id/validate
```

### 4.4 Inventaires

```
GET    /api/inventories
GET    /api/inventories/:id
POST   /api/inventories
PUT    /api/inventories/:id
DELETE /api/inventories/:id
POST   /api/inventories/:id/start     # Démarrer (geler stock)
POST   /api/inventories/:id/count     # Saisir comptage ligne
POST   /api/inventories/:id/validate  # Valider écarts
POST   /api/inventories/:id/adjust    # Appliquer ajustements
POST   /api/inventories/:id/close     # Clôturer
POST   /api/inventories/:id/cancel    # Annuler
GET    /api/inventories/:id/print     # Feuilles de comptage PDF
GET    /api/inventories/:id/report    # Rapport écarts
```

### 4.5 Lots & Séries

```
GET    /api/lots
GET    /api/lots/:id
POST   /api/lots
PUT    /api/lots/:id
PATCH  /api/lots/:id/block           # Bloquer lot
PATCH  /api/lots/:id/unblock         # Débloquer lot
GET    /api/lots/:id/trace-upstream   # Traçabilité ascendante
GET    /api/lots/:id/trace-downstream # Traçabilité descendante
GET    /api/lots/:id/movements        # Mouvements du lot

GET    /api/serial-numbers
GET    /api/serial-numbers/:number
```

### 4.6 Reporting

```
GET    /api/reports/stock-levels      # État des stocks
GET    /api/reports/valuation         # Valorisation
GET    /api/reports/movements         # Mouvements période
GET    /api/reports/inventory-variance # Écarts inventaire
GET    /api/reports/stock-rotation    # Rotation des stocks
GET    /api/reports/alerts            # Alertes (ruptures, DLC)
GET    /api/reports/kpis              # Indicateurs
GET    /api/reports/export/:type      # Export PDF/Excel
```

### 4.7 Configuration

```
GET    /api/categories
POST   /api/categories
PUT    /api/categories/:id
DELETE /api/categories/:id

GET    /api/units-of-measure
POST   /api/units-of-measure
PUT    /api/units-of-measure/:id
DELETE /api/units-of-measure/:id

GET    /api/settings/stock
PUT    /api/settings/stock
```

---

## 5. SYSTÈME DE PERMISSIONS

### 5.1 Middleware de vérification

```python
# /backend/middleware/permissions.py

from functools import wraps
from fastapi import HTTPException, Depends
from typing import List

def require_permissions(*permissions: str):
    """
    Décorateur pour vérifier les permissions utilisateur
    Usage: @require_permissions("article.create", "article.update")
    """
    def decorator(func):
        @wraps(func)
        async def wrapper(*args, current_user = Depends(get_current_user), **kwargs):
            user_permissions = await get_user_permissions(current_user.id)
            
            for permission in permissions:
                if permission not in user_permissions:
                    raise HTTPException(
                        status_code=403,
                        detail=f"Permission refusée: {permission} requise"
                    )
            
            return await func(*args, current_user=current_user, **kwargs)
        return wrapper
    return decorator

def require_any_permission(*permissions: str):
    """
    Vérifie qu'au moins une permission est présente
    """
    # ... implémentation similaire avec logique OR
```

### 5.2 Liste complète des permissions

```python
# /backend/utils/permissions.py

PERMISSIONS = {
    # Articles
    "article.view": "Voir la liste des articles",
    "article.create": "Créer un nouvel article",
    "article.update": "Modifier un article",
    "article.archive": "Archiver un article",
    "article.delete": "Supprimer un article",
    "article.pricing": "Gérer les prix",
    "article.import": "Importer des articles",
    "article.export": "Exporter des articles",
    
    # Catégories
    "category.view": "Voir les catégories",
    "category.manage": "Gérer les catégories",
    
    # Unités de mesure
    "uom.view": "Voir les unités",
    "uom.manage": "Gérer les unités",
    
    # Entrepôts
    "warehouse.view": "Voir les entrepôts",
    "warehouse.create": "Créer un entrepôt",
    "warehouse.update": "Modifier un entrepôt",
    "warehouse.delete": "Supprimer un entrepôt",
    "warehouse.access": "Gérer les accès entrepôt",
    
    # Zones
    "zone.view": "Voir les zones",
    "zone.create": "Créer une zone",
    "zone.update": "Modifier une zone",
    "zone.delete": "Supprimer une zone",
    
    # Emplacements
    "location.view": "Voir les emplacements",
    "location.create": "Créer un emplacement",
    "location.update": "Modifier un emplacement",
    "location.delete": "Supprimer un emplacement",
    "location.print": "Imprimer étiquettes",
    
    # Entrées stock
    "stockEntry.view": "Voir les entrées",
    "stockEntry.create": "Créer une entrée",
    "stockEntry.update": "Modifier une entrée",
    "stockEntry.validate": "Valider une entrée",
    "stockEntry.cancel": "Annuler une entrée",
    "stockEntry.delete": "Supprimer une entrée",
    "stockEntry.print": "Imprimer bordereau entrée",
    "stockEntry.reverse": "Contre-passer entrée",
    
    # Sorties stock
    "stockExit.view": "Voir les sorties",
    "stockExit.create": "Créer une sortie",
    "stockExit.update": "Modifier une sortie",
    "stockExit.validate": "Valider une sortie",
    "stockExit.cancel": "Annuler une sortie",
    "stockExit.delete": "Supprimer une sortie",
    "stockExit.print": "Imprimer bordereau sortie",
    "stockExit.force": "Forcer sortie (stock négatif)",
    "stockExit.reverse": "Contre-passer sortie",
    
    # Transferts
    "transfer.view": "Voir les transferts",
    "transfer.create": "Créer un transfert",
    "transfer.update": "Modifier un transfert",
    "transfer.validate": "Valider transfert (départ)",
    "transfer.receive": "Réceptionner transfert",
    "transfer.cancel": "Annuler transfert",
    
    # Ajustements
    "adjustment.view": "Voir les ajustements",
    "adjustment.create": "Créer un ajustement",
    "adjustment.update": "Modifier un ajustement",
    "adjustment.validate": "Valider un ajustement",
    "adjustment.cancel": "Annuler un ajustement",
    "adjustment.delete": "Supprimer un ajustement",
    
    # Inventaires
    "inventory.view": "Voir les inventaires",
    "inventory.create": "Créer un inventaire",
    "inventory.count": "Saisir comptages",
    "inventory.recount": "Double comptage",
    "inventory.validate": "Valider écarts",
    "inventory.adjust": "Appliquer ajustements",
    "inventory.close": "Clôturer inventaire",
    "inventory.cancel": "Annuler inventaire",
    "inventory.freeze": "Geler le stock",
    "inventory.print": "Imprimer feuilles comptage",
    
    # Lots
    "lot.view": "Voir les lots",
    "lot.create": "Créer un lot",
    "lot.update": "Modifier un lot",
    "lot.block": "Bloquer/débloquer lot",
    
    # Séries
    "serial.view": "Voir les numéros de série",
    "serial.manage": "Gérer les séries",
    
    # Historique & Traçabilité
    "history.view": "Voir l'historique",
    "history.export": "Exporter l'historique",
    "trace.view": "Voir la traçabilité",
    "audit.view": "Voir le journal d'audit",
    "audit.export": "Exporter l'audit",
    
    # Reporting
    "report.stock": "Rapport état stocks",
    "report.valuation": "Rapport valorisation",
    "report.variance": "Rapport écarts",
    "report.kpi": "Rapport KPIs",
    "report.export": "Exporter rapports",
    "report.schedule": "Programmer rapports",
    
    # Dashboard
    "dashboard.view": "Voir tableaux de bord",
    "dashboard.create": "Créer tableau de bord",
    
    # Alertes
    "alert.config": "Configurer alertes",
    "alert.receive": "Recevoir alertes",
    
    # Administration
    "settings.stock": "Paramètres du module",
    "settings.valuation": "Paramètres valorisation",
    "settings.workflow": "Paramètres workflow",
    "role.view": "Voir les rôles",
    "role.manage": "Gérer les rôles",
    "permission.assign": "Attribuer permissions",
    "user.warehouse.assign": "Affecter utilisateurs aux entrepôts"
}
```

### 5.3 Rôles prédéfinis

```python
# /backend/utils/roles.py

ROLES = {
    "SUPER_ADMIN": {
        "name": "Super Administrateur",
        "level": 100,
        "permissions": ["*"]  # Toutes les permissions
    },
    
    "STOCK_ADMIN": {
        "name": "Administrateur Stock",
        "level": 90,
        "permissions": [
            # Toutes sauf role.manage et certaines admin
            "article.*", "category.*", "uom.*",
            "warehouse.*", "zone.*", "location.*",
            "stockEntry.*", "stockExit.*", "transfer.*",
            "adjustment.*", "inventory.*",
            "lot.*", "serial.*",
            "history.*", "trace.*", "audit.*",
            "report.*", "dashboard.*", "alert.*",
            "settings.*", "role.view", "permission.assign",
            "user.warehouse.assign"
        ]
    },
    
    "WAREHOUSE_MANAGER": {
        "name": "Responsable Entrepôt",
        "level": 80,
        "permissions": [
            "article.view", "article.create", "article.update",
            "article.archive", "article.export",
            "category.view", "uom.view",
            "warehouse.view", "warehouse.update",
            "zone.*", "location.*",
            "stockEntry.*", "stockExit.view", "stockExit.create",
            "stockExit.update", "stockExit.validate", "stockExit.cancel",
            "stockExit.delete", "stockExit.print", "stockExit.reverse",
            "transfer.*", "adjustment.view", "adjustment.create",
            "adjustment.update", "adjustment.validate", "adjustment.cancel",
            "adjustment.delete",
            "inventory.*",
            "lot.*", "serial.*",
            "history.*", "trace.view", "audit.view",
            "report.*", "dashboard.*", "alert.*"
        ]
    },
    
    "TEAM_LEADER": {
        "name": "Chef d'équipe",
        "level": 70,
        "permissions": [
            "article.view", "article.export",
            "category.view", "uom.view",
            "warehouse.view", "zone.view", "location.*",
            "stockEntry.view", "stockEntry.create", "stockEntry.update",
            "stockEntry.validate", "stockEntry.cancel", "stockEntry.print",
            "stockExit.view", "stockExit.create", "stockExit.update",
            "stockExit.validate", "stockExit.cancel", "stockExit.print",
            "transfer.view", "transfer.create", "transfer.update",
            "transfer.validate", "transfer.receive", "transfer.cancel",
            "adjustment.view", "adjustment.create", "adjustment.update",
            "adjustment.validate", "adjustment.cancel",
            "inventory.view", "inventory.create", "inventory.count",
            "inventory.recount", "inventory.validate", "inventory.print",
            "lot.*", "serial.view", "serial.manage",
            "history.view", "history.export", "trace.view",
            "report.stock", "report.valuation", "report.variance",
            "report.kpi", "report.export",
            "dashboard.view", "dashboard.create", "alert.config", "alert.receive"
        ]
    },
    
    "SENIOR_OPERATOR": {
        "name": "Magasinier Senior",
        "level": 60,
        "permissions": [
            "article.view", "article.export",
            "category.view", "uom.view",
            "warehouse.view", "zone.view", "location.view", "location.print",
            "stockEntry.view", "stockEntry.create", "stockEntry.update",
            "stockEntry.validate", "stockEntry.cancel", "stockEntry.print",
            "stockExit.view", "stockExit.create", "stockExit.update",
            "stockExit.validate", "stockExit.cancel", "stockExit.print",
            "transfer.view", "transfer.create", "transfer.update",
            "transfer.validate", "transfer.receive",
            "adjustment.view", "adjustment.create", "adjustment.update",
            "inventory.view", "inventory.count", "inventory.recount",
            "inventory.print",
            "lot.view", "lot.create", "lot.update",
            "serial.view", "serial.manage",
            "history.view", "trace.view",
            "report.stock", "report.variance", "report.export",
            "dashboard.view", "alert.receive"
        ]
    },
    
    "OPERATOR": {
        "name": "Magasinier",
        "level": 50,
        "permissions": [
            "article.view",
            "category.view", "uom.view",
            "warehouse.view", "zone.view", "location.view", "location.print",
            "stockEntry.view", "stockEntry.create", "stockEntry.print",
            "stockExit.view", "stockExit.create", "stockExit.print",
            "transfer.view", "transfer.create", "transfer.receive",
            "adjustment.view",
            "inventory.view", "inventory.count", "inventory.print",
            "lot.view", "serial.view",
            "history.view", "trace.view",
            "report.stock",
            "dashboard.view", "alert.receive"
        ]
    },
    
    "INVENTORY_CLERK": {
        "name": "Inventoriste",
        "level": 50,
        "permissions": [
            "article.view",
            "category.view", "uom.view",
            "warehouse.view", "zone.view", "location.view",
            "stockEntry.view", "stockExit.view", "transfer.view",
            "adjustment.view", "adjustment.create", "adjustment.update",
            "inventory.view", "inventory.create", "inventory.count",
            "inventory.recount", "inventory.print",
            "lot.view", "serial.view",
            "history.view", "trace.view",
            "report.stock", "report.variance", "report.export",
            "dashboard.view", "alert.receive"
        ]
    },
    
    "QUALITY_CONTROLLER": {
        "name": "Contrôleur Qualité",
        "level": 60,
        "permissions": [
            "article.view",
            "category.view", "uom.view",
            "warehouse.view", "zone.view", "location.view",
            "stockEntry.view", "stockExit.view", "transfer.view",
            "adjustment.view",
            "inventory.view",
            "lot.view", "lot.create", "lot.update", "lot.block",
            "serial.view",
            "history.view", "history.export", "trace.view", "audit.view",
            "report.stock", "report.variance", "report.export",
            "dashboard.view", "alert.receive"
        ]
    },
    
    "ANALYST": {
        "name": "Analyste",
        "level": 40,
        "permissions": [
            "article.view", "article.export",
            "category.view", "uom.view",
            "warehouse.view", "zone.view", "location.view",
            "stockEntry.view", "stockExit.view", "transfer.view",
            "adjustment.view",
            "inventory.view",
            "lot.view", "serial.view",
            "history.view", "history.export", "trace.view",
            "audit.view", "audit.export",
            "report.*",
            "dashboard.view", "dashboard.create",
            "alert.receive"
        ]
    },
    
    "VIEWER": {
        "name": "Consultant",
        "level": 10,
        "permissions": [
            "article.view",
            "category.view", "uom.view",
            "warehouse.view", "zone.view", "location.view",
            "stockEntry.view", "stockExit.view", "transfer.view",
            "adjustment.view",
            "inventory.view",
            "lot.view", "serial.view",
            "history.view", "trace.view",
            "report.stock",
            "dashboard.view"
        ]
    }
}
```

### 5.4 Hook React pour les permissions

```javascript
// /frontend/src/hooks/usePermissions.js

import { useAuth } from './useAuth';

export const usePermissions = () => {
  const { user } = useAuth();
  
  const hasPermission = (permission) => {
    if (!user?.permissions) return false;
    if (user.permissions.includes('*')) return true;
    
    // Support wildcard (article.*)
    const parts = permission.split('.');
    if (parts.length === 2) {
      const wildcard = `${parts[0]}.*`;
      if (user.permissions.includes(wildcard)) return true;
    }
    
    return user.permissions.includes(permission);
  };
  
  const hasAnyPermission = (...permissions) => {
    return permissions.some(p => hasPermission(p));
  };
  
  const hasAllPermissions = (...permissions) => {
    return permissions.every(p => hasPermission(p));
  };
  
  return { hasPermission, hasAnyPermission, hasAllPermissions };
};

// Composant conditionnel
export const Can = ({ permission, permissions, any = false, children, fallback = null }) => {
  const { hasPermission, hasAnyPermission, hasAllPermissions } = usePermissions();
  
  let allowed = false;
  
  if (permission) {
    allowed = hasPermission(permission);
  } else if (permissions) {
    allowed = any ? hasAnyPermission(...permissions) : hasAllPermissions(...permissions);
  }
  
  return allowed ? children : fallback;
};
```

---

## 6. RÈGLES MÉTIER

### 6.1 Règles de validation des mouvements

```python
# /backend/services/stock_service.py

class StockValidationRules:
    
    @staticmethod
    def validate_exit(movement: StockMovement, stock_levels: dict) -> tuple[bool, str]:
        """
        Valide une sortie de stock
        Returns: (is_valid, error_message)
        """
        for line in movement.lines:
            available = stock_levels.get(line.article_id, {}).get('available', 0)
            
            if line.quantity > available:
                # Vérifier si force autorisé
                if not movement.force_negative:
                    return False, f"Stock insuffisant pour {line.article_id}: demandé {line.quantity}, disponible {available}"
        
        return True, None
    
    @staticmethod
    def validate_lot_status(lot: Lot) -> tuple[bool, str]:
        """
        Vérifie si un lot peut être utilisé
        """
        if lot.status == 'blocked':
            return False, f"Lot {lot.number} bloqué: {lot.block_reason}"
        
        if lot.status == 'quarantine':
            return False, f"Lot {lot.number} en quarantaine"
        
        if lot.status == 'expired':
            return False, f"Lot {lot.number} périmé"
        
        if lot.expiry_date and lot.expiry_date < datetime.now():
            return False, f"Lot {lot.number} date d'expiration dépassée"
        
        return True, None
    
    @staticmethod
    def validate_location_capacity(location: Location, quantity: float, article: Article) -> tuple[bool, str]:
        """
        Vérifie la capacité de l'emplacement
        """
        if location.max_weight and (quantity * article.unit_weight) > location.max_weight:
            return False, f"Capacité poids dépassée pour {location.code}"
        
        if location.max_volume and (quantity * article.unit_volume) > location.max_volume:
            return False, f"Capacité volume dépassée pour {location.code}"
        
        return True, None
```

### 6.2 Règles de numérotation automatique

```python
# /backend/utils/sequence.py

class SequenceGenerator:
    """
    Génère les numéros de référence automatiques
    Format: PREFIX-YYYYMM-XXXXX
    """
    
    PREFIXES = {
        'stock_entry': 'ENT',
        'stock_exit': 'SOR',
        'transfer': 'TRF',
        'adjustment': 'AJU',
        'inventory': 'INV',
        'lot': 'LOT'
    }
    
    @classmethod
    async def generate(cls, document_type: str, db) -> str:
        prefix = cls.PREFIXES.get(document_type, 'DOC')
        year_month = datetime.now().strftime('%Y%m')
        
        # Récupérer et incrémenter le compteur
        counter = await db.sequences.find_one_and_update(
            {'type': document_type, 'period': year_month},
            {'$inc': {'value': 1}},
            upsert=True,
            return_document=ReturnDocument.AFTER
        )
        
        sequence = str(counter['value']).zfill(5)
        return f"{prefix}-{year_month}-{sequence}"
```

### 6.3 Règles de valorisation

```python
# /backend/services/valuation_service.py

class ValuationService:
    
    @staticmethod
    def calculate_fifo(article_id: str, quantity: float, movements: list) -> float:
        """
        Calcul FIFO: First In First Out
        """
        total_cost = 0
        remaining = quantity
        
        # Trier par date d'entrée croissante
        entries = sorted(
            [m for m in movements if m.type == 'entry'],
            key=lambda x: x.effective_date
        )
        
        for entry in entries:
            if remaining <= 0:
                break
            
            for line in entry.lines:
                if line.article_id == article_id and remaining > 0:
                    qty_to_use = min(remaining, line.quantity)
                    total_cost += qty_to_use * line.unit_cost
                    remaining -= qty_to_use
        
        return total_cost
    
    @staticmethod
    def calculate_lifo(article_id: str, quantity: float, movements: list) -> float:
        """
        Calcul LIFO: Last In First Out
        """
        # Même logique mais tri décroissant
        entries = sorted(
            [m for m in movements if m.type == 'entry'],
            key=lambda x: x.effective_date,
            reverse=True
        )
        # ... suite similaire
    
    @staticmethod
    def calculate_average_cost(article_id: str, db) -> float:
        """
        Calcul Coût Moyen Pondéré
        """
        pipeline = [
            {'$match': {'article_id': article_id, 'type': 'entry', 'status': 'validated'}},
            {'$unwind': '$lines'},
            {'$match': {'lines.article_id': article_id}},
            {'$group': {
                '_id': None,
                'total_qty': {'$sum': '$lines.quantity'},
                'total_value': {'$sum': {'$multiply': ['$lines.quantity', '$lines.unit_cost']}}
            }}
        ]
        
        result = db.stock_movements.aggregate(pipeline)
        if result:
            return result['total_value'] / result['total_qty']
        return 0
```

### 6.4 Règles d'alerte

```python
# /backend/services/alert_service.py

class AlertService:
    
    ALERT_TYPES = {
        'LOW_STOCK': 'Stock bas',
        'OUT_OF_STOCK': 'Rupture de stock',
        'OVERSTOCK': 'Sur-stock',
        'EXPIRING_SOON': 'Péremption proche',
        'EXPIRED': 'Produit périmé',
        'BLOCKED_LOT': 'Lot bloqué'
    }
    
    @classmethod
    async def check_stock_alerts(cls, article_id: str, db):
        """
        Vérifie et génère les alertes de stock
        """
        article = await db.articles.find_one({'_id': article_id})
        stock = await db.stock_levels.aggregate([
            {'$match': {'article_id': article_id}},
            {'$group': {'_id': None, 'total': {'$sum': '$quantity_available'}}}
        ])
        
        total_available = stock.get('total', 0) if stock else 0
        
        alerts = []
        
        if total_available <= 0:
            alerts.append(cls._create_alert('OUT_OF_STOCK', article))
        elif total_available <= article.get('security_stock', 0):
            alerts.append(cls._create_alert('LOW_STOCK', article))
        elif article.get('max_stock') and total_available > article['max_stock']:
            alerts.append(cls._create_alert('OVERSTOCK', article))
        
        return alerts
    
    @classmethod
    async def check_expiry_alerts(cls, db, days_threshold: int = 30):
        """
        Vérifie les lots proches de la péremption
        """
        threshold_date = datetime.now() + timedelta(days=days_threshold)
        
        expiring_lots = await db.lots.find({
            'status': {'$nin': ['expired', 'blocked']},
            'expiry_date': {'$lte': threshold_date, '$gte': datetime.now()}
        }).to_list(None)
        
        alerts = []
        for lot in expiring_lots:
            alerts.append(cls._create_alert('EXPIRING_SOON', lot))
        
        return alerts
```

---

## 7. COMPOSANTS UI

### 7.1 Structure des pages

```
/pages
├── /articles
│   ├── index.jsx              # Liste des articles
│   ├── [id].jsx               # Détail/édition article
│   └── create.jsx             # Création article
│
├── /warehouses
│   ├── index.jsx              # Liste entrepôts
│   ├── [id]/
│   │   ├── index.jsx          # Détail entrepôt
│   │   ├── locations.jsx      # Emplacements
│   │   └── stock.jsx          # Stock de l'entrepôt
│
├── /operations
│   ├── entries/
│   │   ├── index.jsx          # Liste entrées
│   │   ├── [id].jsx           # Détail entrée
│   │   └── create.jsx         # Nouvelle entrée
│   ├── exits/
│   ├── transfers/
│   └── adjustments/
│
├── /inventory
│   ├── index.jsx              # Liste inventaires
│   ├── [id]/
│   │   ├── index.jsx          # Détail inventaire
│   │   ├── count.jsx          # Saisie comptage
│   │   └── variance.jsx       # Écarts
│
├── /traceability
│   ├── lots/
│   ├── serials/
│   └── history.jsx
│
├── /reports
│   ├── stock-levels.jsx
│   ├── valuation.jsx
│   ├── movements.jsx
│   └── dashboard.jsx
│
└── /settings
    ├── categories.jsx
    ├── units.jsx
    ├── roles.jsx
    └── general.jsx
```

### 7.2 Composants réutilisables

```javascript
// Composants essentiels à créer

// 1. Tableau de données avec filtres, tri, pagination
<DataTable
  columns={columns}
  data={data}
  filters={filters}
  onFilter={handleFilter}
  pagination={pagination}
  onPageChange={handlePageChange}
  selectable
  onSelectionChange={handleSelection}
  actions={rowActions}
/>

// 2. Formulaire de mouvement de stock
<StockMovementForm
  type="entry" // 'entry' | 'exit' | 'transfer' | 'adjustment'
  initialData={movement}
  onSubmit={handleSubmit}
  onValidate={handleValidate}
  readOnly={isValidated}
/>

// 3. Sélecteur d'article avec recherche
<ArticleSelector
  value={selectedArticle}
  onChange={setSelectedArticle}
  warehouseId={warehouseId}
  showStock={true}
  multiple={false}
/>

// 4. Sélecteur d'emplacement hiérarchique
<LocationSelector
  warehouseId={warehouseId}
  value={location}
  onChange={setLocation}
  zoneFilter="storage"
/>

// 5. Saisie de lot/série
<LotSerialInput
  articleId={articleId}
  trackingType={article.trackingType}
  value={lotSerial}
  onChange={setLotSerial}
  quantity={quantity}
/>

// 6. Indicateur de stock
<StockIndicator
  articleId={articleId}
  warehouseId={warehouseId}
  showDetails={true}
/>

// 7. Timeline des mouvements
<MovementTimeline
  entityType="article" // 'article' | 'lot' | 'location'
  entityId={id}
  limit={10}
/>

// 8. Badge de statut
<StatusBadge
  status={status}
  type="movement" // 'movement' | 'inventory' | 'lot'
/>

// 9. Widget d'alerte
<AlertWidget
  type="stock"
  filters={{ warehouseId }}
  onAlertClick={handleAlertClick}
/>

// 10. Scanner code-barres
<BarcodeScanner
  onScan={handleScan}
  onError={handleError}
  continuous={false}
/>
```

### 7.3 États et transitions UI

```javascript
// États des mouvements de stock
const MOVEMENT_STATUSES = {
  draft: {
    label: 'Brouillon',
    color: 'gray',
    icon: 'FileEdit',
    actions: ['edit', 'delete', 'validate']
  },
  pending: {
    label: 'En attente',
    color: 'yellow',
    icon: 'Clock',
    actions: ['validate', 'cancel']
  },
  validated: {
    label: 'Validé',
    color: 'green',
    icon: 'CheckCircle',
    actions: ['reverse', 'print']
  },
  cancelled: {
    label: 'Annulé',
    color: 'red',
    icon: 'XCircle',
    actions: ['view']
  },
  reversed: {
    label: 'Contre-passé',
    color: 'orange',
    icon: 'RotateCcw',
    actions: ['view']
  }
};

// États des inventaires
const INVENTORY_STATUSES = {
  draft: {
    label: 'Brouillon',
    color: 'gray',
    nextStates: ['in_progress', 'cancelled']
  },
  in_progress: {
    label: 'En cours',
    color: 'blue',
    nextStates: ['counting', 'cancelled']
  },
  counting: {
    label: 'Comptage',
    color: 'yellow',
    nextStates: ['validation', 'cancelled']
  },
  validation: {
    label: 'Validation',
    color: 'orange',
    nextStates: ['closed', 'counting']
  },
  closed: {
    label: 'Clôturé',
    color: 'green',
    nextStates: []
  },
  cancelled: {
    label: 'Annulé',
    color: 'red',
    nextStates: []
  }
};
```

---

## 8. WORKFLOWS & ÉTATS

### 8.1 Machine à états - Mouvement de stock

```python
# /backend/services/state_machine.py

from enum import Enum
from typing import Callable, Dict, List

class MovementStatus(str, Enum):
    DRAFT = 'draft'
    PENDING = 'pending'
    VALIDATED = 'validated'
    CANCELLED = 'cancelled'
    REVERSED = 'reversed'

class MovementStateMachine:
    """
    Gestion des transitions d'état pour les mouvements de stock
    """
    
    TRANSITIONS = {
        MovementStatus.DRAFT: [MovementStatus.PENDING, MovementStatus.VALIDATED, MovementStatus.CANCELLED],
        MovementStatus.PENDING: [MovementStatus.VALIDATED, MovementStatus.CANCELLED],
        MovementStatus.VALIDATED: [MovementStatus.REVERSED],
        MovementStatus.CANCELLED: [],
        MovementStatus.REVERSED: []
    }
    
    REQUIRED_PERMISSIONS = {
        (MovementStatus.DRAFT, MovementStatus.VALIDATED): 'stockEntry.validate',
        (MovementStatus.DRAFT, MovementStatus.CANCELLED): 'stockEntry.cancel',
        (MovementStatus.PENDING, MovementStatus.VALIDATED): 'stockEntry.validate',
        (MovementStatus.PENDING, MovementStatus.CANCELLED): 'stockEntry.cancel',
        (MovementStatus.VALIDATED, MovementStatus.REVERSED): 'stockEntry.reverse'
    }
    
    @classmethod
    def can_transition(cls, current: MovementStatus, target: MovementStatus) -> bool:
        return target in cls.TRANSITIONS.get(current, [])
    
    @classmethod
    def get_required_permission(cls, current: MovementStatus, target: MovementStatus) -> str:
        return cls.REQUIRED_PERMISSIONS.get((current, target))
    
    @classmethod
    async def transition(
        cls,
        movement: dict,
        target_status: MovementStatus,
        user: dict,
        db,
        hooks: Dict[str, Callable] = None
    ):
        """
        Effectue la transition d'état avec validation et hooks
        """
        current = MovementStatus(movement['status'])
        
        # Vérifier la transition
        if not cls.can_transition(current, target_status):
            raise ValueError(f"Transition invalide: {current} -> {target_status}")
        
        # Vérifier la permission
        required_perm = cls.get_required_permission(current, target_status)
        if required_perm and required_perm not in user.get('permissions', []):
            raise PermissionError(f"Permission requise: {required_perm}")
        
        # Hook pre-transition
        if hooks and 'pre_transition' in hooks:
            await hooks['pre_transition'](movement, target_status)
        
        # Actions spécifiques par transition
        if target_status == MovementStatus.VALIDATED:
            await cls._on_validate(movement, db)
        elif target_status == MovementStatus.REVERSED:
            await cls._on_reverse(movement, db)
        
        # Mettre à jour le statut
        await db.stock_movements.update_one(
            {'_id': movement['_id']},
            {
                '$set': {
                    'status': target_status.value,
                    f'{target_status.value}_at': datetime.now(),
                    f'{target_status.value}_by': user['_id']
                }
            }
        )
        
        # Hook post-transition
        if hooks and 'post_transition' in hooks:
            await hooks['post_transition'](movement, target_status)
        
        # Logger l'audit
        await cls._log_transition(movement, current, target_status, user, db)
    
    @classmethod
    async def _on_validate(cls, movement: dict, db):
        """
        Actions lors de la validation
        """
        for line in movement['lines']:
            # Mettre à jour le stock
            update_qty = line['quantity'] if movement['type'] == 'entry' else -line['quantity']
            
            await db.stock_levels.update_one(
                {
                    'article_id': line['article_id'],
                    'warehouse_id': movement['dest_warehouse_id'] or movement['source_warehouse_id'],
                    'location_id': line.get('location_id'),
                    'lot_id': line.get('lot_id')
                },
                {
                    '$inc': {'quantity_on_hand': update_qty, 'quantity_available': update_qty},
                    '$set': {'last_movement_at': datetime.now()}
                },
                upsert=True
            )
    
    @classmethod
    async def _on_reverse(cls, movement: dict, db):
        """
        Actions lors de la contre-passation
        """
        # Créer un mouvement inverse
        reverse_movement = {
            **movement,
            '_id': ObjectId(),
            'reference': await SequenceGenerator.generate(movement['type'], db),
            'status': MovementStatus.VALIDATED.value,
            'related_document_type': 'reversal',
            'related_document_id': movement['_id']
        }
        
        # Inverser les quantités
        for line in reverse_movement['lines']:
            line['quantity'] = -line['quantity']
        
        await db.stock_movements.insert_one(reverse_movement)
        
        # Mettre à jour le stock (inverse)
        # ... même logique que _on_validate avec signe inverse
```

### 8.2 Machine à états - Inventaire

```python
class InventoryStatus(str, Enum):
    DRAFT = 'draft'
    IN_PROGRESS = 'in_progress'
    COUNTING = 'counting'
    VALIDATION = 'validation'
    CLOSED = 'closed'
    CANCELLED = 'cancelled'

class InventoryStateMachine:
    
    TRANSITIONS = {
        InventoryStatus.DRAFT: [InventoryStatus.IN_PROGRESS, InventoryStatus.CANCELLED],
        InventoryStatus.IN_PROGRESS: [InventoryStatus.COUNTING, InventoryStatus.CANCELLED],
        InventoryStatus.COUNTING: [InventoryStatus.VALIDATION, InventoryStatus.CANCELLED],
        InventoryStatus.VALIDATION: [InventoryStatus.CLOSED, InventoryStatus.COUNTING],
        InventoryStatus.CLOSED: [],
        InventoryStatus.CANCELLED: []
    }
    
    @classmethod
    async def start_inventory(cls, inventory: dict, db):
        """
        Démarre l'inventaire - gèle le stock
        """
        # Geler les emplacements concernés
        await db.locations.update_many(
            {'_id': {'$in': inventory.get('location_ids', [])}},
            {'$set': {'frozen': True, 'frozen_by_inventory': inventory['_id']}}
        )
        
        # Générer les lignes de comptage
        stock_levels = await db.stock_levels.find({
            'location_id': {'$in': inventory.get('location_ids', [])}
        }).to_list(None)
        
        lines = []
        for level in stock_levels:
            lines.append({
                'article_id': level['article_id'],
                'location_id': level['location_id'],
                'lot_id': level.get('lot_id'),
                'theoretical_quantity': level['quantity_on_hand'],
                'counted_quantity': None,
                'variance_quantity': None,
                'status': 'pending'
            })
        
        await db.inventories.update_one(
            {'_id': inventory['_id']},
            {'$set': {'lines': lines, 'status': InventoryStatus.IN_PROGRESS.value}}
        )
    
    @classmethod
    async def close_inventory(cls, inventory: dict, apply_adjustments: bool, db, user: dict):
        """
        Clôture l'inventaire et applique les ajustements si demandé
        """
        if apply_adjustments:
            # Créer les ajustements pour les écarts
            for line in inventory['lines']:
                if line['variance_quantity'] and line['variance_quantity'] != 0:
                    adjustment = {
                        'type': 'adjustment',
                        'subtype': 'inventory',
                        'status': 'validated',
                        'source_warehouse_id': inventory['warehouse_id'],
                        'lines': [{
                            'article_id': line['article_id'],
                            'location_id': line['location_id'],
                            'lot_id': line.get('lot_id'),
                            'quantity': line['variance_quantity'],
                            'reason': 'Ajustement inventaire'
                        }],
                        'related_document_type': 'inventory',
                        'related_document_id': inventory['_id'],
                        'created_at': datetime.now(),
                        'created_by': user['_id'],
                        'validated_at': datetime.now(),
                        'validated_by': user['_id']
                    }
                    
                    await db.stock_movements.insert_one(adjustment)
                    
                    # Mettre à jour le stock
                    await db.stock_levels.update_one(
                        {
                            'article_id': line['article_id'],
                            'location_id': line['location_id'],
                            'lot_id': line.get('lot_id')
                        },
                        {
                            '$inc': {
                                'quantity_on_hand': line['variance_quantity'],
                                'quantity_available': line['variance_quantity']
                            },
                            '$set': {'last_inventory_at': datetime.now()}
                        }
                    )
        
        # Dégeler les emplacements
        await db.locations.update_many(
            {'frozen_by_inventory': inventory['_id']},
            {'$set': {'frozen': False}, '$unset': {'frozen_by_inventory': ''}}
        )
        
        # Mettre à jour le statut
        await db.inventories.update_one(
            {'_id': inventory['_id']},
            {
                '$set': {
                    'status': InventoryStatus.CLOSED.value,
                    'closed_at': datetime.now(),
                    'closed_by': user['_id']
                }
            }
        )
```

---

## 9. TESTS REQUIS

### 9.1 Tests unitaires backend

```python
# /tests/unit/test_stock_service.py

import pytest
from services.stock_service import StockService
from services.valuation_service import ValuationService

class TestStockValidation:
    
    def test_exit_with_sufficient_stock(self):
        """Sortie avec stock suffisant doit réussir"""
        stock_levels = {'article_1': {'available': 100}}
        movement = create_movement(type='exit', lines=[
            {'article_id': 'article_1', 'quantity': 50}
        ])
        
        is_valid, error = StockValidationRules.validate_exit(movement, stock_levels)
        assert is_valid is True
        assert error is None
    
    def test_exit_with_insufficient_stock(self):
        """Sortie avec stock insuffisant doit échouer"""
        stock_levels = {'article_1': {'available': 30}}
        movement = create_movement(type='exit', lines=[
            {'article_id': 'article_1', 'quantity': 50}
        ])
        
        is_valid, error = StockValidationRules.validate_exit(movement, stock_levels)
        assert is_valid is False
        assert 'Stock insuffisant' in error
    
    def test_blocked_lot_cannot_be_used(self):
        """Lot bloqué ne peut pas être utilisé"""
        lot = create_lot(status='blocked', block_reason='Qualité')
        
        is_valid, error = StockValidationRules.validate_lot_status(lot)
        assert is_valid is False
        assert 'bloqué' in error


class TestValuation:
    
    def test_fifo_calculation(self):
        """Calcul FIFO correct"""
        movements = [
            create_entry(date='2024-01-01', quantity=10, unit_cost=100),
            create_entry(date='2024-01-15', quantity=10, unit_cost=120),
        ]
        
        cost = ValuationService.calculate_fifo('article_1', 15, movements)
        # 10 * 100 + 5 * 120 = 1600
        assert cost == 1600
    
    def test_average_cost_calculation(self):
        """Calcul coût moyen correct"""
        # ...
```

### 9.2 Tests d'intégration API

```python
# /tests/integration/test_stock_entries_api.py

import pytest
from httpx import AsyncClient

class TestStockEntriesAPI:
    
    @pytest.fixture
    async def authenticated_client(self):
        # Setup client avec token
        pass
    
    async def test_create_entry_success(self, authenticated_client):
        """Création d'une entrée en stock"""
        response = await authenticated_client.post('/api/stock-entries', json={
            'warehouse_id': 'wh_1',
            'lines': [
                {'article_id': 'art_1', 'quantity': 10, 'unit_cost': 100}
            ]
        })
        
        assert response.status_code == 201
        assert response.json()['status'] == 'draft'
        assert response.json()['reference'].startswith('ENT-')
    
    async def test_validate_entry_updates_stock(self, authenticated_client):
        """Validation d'entrée met à jour le stock"""
        # Créer entrée
        entry = await create_entry(authenticated_client)
        
        # Valider
        response = await authenticated_client.post(
            f'/api/stock-entries/{entry["_id"]}/validate'
        )
        
        assert response.status_code == 200
        
        # Vérifier stock mis à jour
        stock = await authenticated_client.get(
            f'/api/articles/{entry["lines"][0]["article_id"]}/stock'
        )
        assert stock.json()['quantity_on_hand'] == 10
    
    async def test_create_entry_without_permission_fails(self, viewer_client):
        """Création sans permission échoue"""
        response = await viewer_client.post('/api/stock-entries', json={...})
        
        assert response.status_code == 403
```

### 9.3 Tests E2E frontend

```javascript
// /tests/e2e/stock-entry.spec.js

describe('Stock Entry Flow', () => {
  
  beforeEach(() => {
    cy.login('warehouse_manager');
  });
  
  it('should create and validate a stock entry', () => {
    // Naviguer vers les entrées
    cy.visit('/operations/entries');
    cy.get('[data-testid="create-entry-btn"]').click();
    
    // Sélectionner entrepôt
    cy.get('[data-testid="warehouse-select"]').click();
    cy.get('[data-testid="warehouse-option-wh1"]').click();
    
    // Ajouter ligne
    cy.get('[data-testid="add-line-btn"]').click();
    cy.get('[data-testid="article-select-0"]').type('ART001');
    cy.get('[data-testid="article-option-0"]').first().click();
    cy.get('[data-testid="quantity-input-0"]').type('10');
    cy.get('[data-testid="unit-cost-input-0"]').type('100');
    
    // Sauvegarder
    cy.get('[data-testid="save-btn"]').click();
    cy.get('[data-testid="status-badge"]').should('contain', 'Brouillon');
    
    // Valider
    cy.get('[data-testid="validate-btn"]').click();
    cy.get('[data-testid="confirm-dialog-btn"]').click();
    cy.get('[data-testid="status-badge"]').should('contain', 'Validé');
    
    // Vérifier stock
    cy.visit('/articles/ART001');
    cy.get('[data-testid="stock-quantity"]').should('contain', '10');
  });
  
  it('should show error when stock is insufficient for exit', () => {
    cy.visit('/operations/exits/create');
    
    // Créer sortie avec quantité > stock
    cy.get('[data-testid="article-select-0"]').type('ART002');
    cy.get('[data-testid="quantity-input-0"]').type('9999');
    
    cy.get('[data-testid="validate-btn"]').click();
    cy.get('[data-testid="error-message"]').should('contain', 'Stock insuffisant');
  });
});
```

---

## 10. CHECKLIST DE DÉVELOPPEMENT

### Phase 1 - Fondations

```markdown
## Sprint 1: Authentification & Base

### Backend
- [ ] Setup FastAPI avec structure de projet
- [ ] Modèle User avec authentification JWT
- [ ] Middleware de permissions
- [ ] CRUD Catégories
- [ ] CRUD Unités de mesure
- [ ] Tests unitaires auth

### Frontend
- [ ] Setup React avec routing
- [ ] Page login
- [ ] Layout principal avec sidebar
- [ ] Hook useAuth et usePermissions
- [ ] Composant Can pour permissions
- [ ] Pages catégories et unités

### Tests
- [ ] Tests auth API
- [ ] Tests permissions
- [ ] Tests E2E login

---

## Sprint 2: Référentiel

### Backend
- [ ] Modèle Article complet
- [ ] CRUD Articles avec filtres/pagination
- [ ] Upload images
- [ ] Import/Export CSV
- [ ] Modèle Warehouse
- [ ] CRUD Warehouses
- [ ] Modèle Location
- [ ] CRUD Locations
- [ ] Tests unitaires

### Frontend
- [ ] Liste articles avec DataTable
- [ ] Formulaire article
- [ ] Gestion images
- [ ] Import/Export
- [ ] Liste entrepôts
- [ ] Formulaire entrepôt
- [ ] Liste emplacements
- [ ] Sélecteur emplacement hiérarchique

### Tests
- [ ] Tests CRUD articles
- [ ] Tests import/export
- [ ] Tests E2E articles
```

### Phase 2 - Opérations

```markdown
## Sprint 3: Entrées & Sorties

### Backend
- [ ] Modèle StockMovement
- [ ] Modèle StockLevel
- [ ] Service StockService
- [ ] API Entrées (CRUD + validate)
- [ ] API Sorties (CRUD + validate)
- [ ] Générateur de séquences
- [ ] Machine à états mouvements
- [ ] Mise à jour stock temps réel
- [ ] Tests unitaires

### Frontend
- [ ] Liste entrées
- [ ] Formulaire entrée avec lignes
- [ ] Sélecteur article avec stock
- [ ] Validation entrée
- [ ] Liste sorties
- [ ] Formulaire sortie
- [ ] Validation sortie
- [ ] Workflow validation

### Tests
- [ ] Tests création/validation entrées
- [ ] Tests mise à jour stock
- [ ] Tests E2E flux complet

---

## Sprint 4: Transferts & Ajustements

### Backend
- [ ] API Transferts
- [ ] Logique transit (départ/arrivée)
- [ ] API Ajustements
- [ ] Contre-passation
- [ ] Tests unitaires

### Frontend
- [ ] Liste/formulaire transferts
- [ ] Sélecteur entrepôt source/destination
- [ ] Liste/formulaire ajustements
- [ ] Motifs d'ajustement
- [ ] Historique mouvements

### Tests
- [ ] Tests transferts
- [ ] Tests ajustements
- [ ] Tests E2E
```

### Phase 3 - Inventaires

```markdown
## Sprint 5: Inventaires

### Backend
- [ ] Modèle Inventory
- [ ] Machine à états inventaire
- [ ] Génération lignes de comptage
- [ ] Gel/dégel stock
- [ ] Double comptage
- [ ] Calcul écarts
- [ ] Application ajustements
- [ ] Génération PDF feuilles comptage
- [ ] Tests unitaires

### Frontend
- [ ] Liste inventaires
- [ ] Création inventaire (périmètre)
- [ ] Interface de comptage
- [ ] Vue écarts
- [ ] Validation écarts
- [ ] Clôture inventaire
- [ ] Impression feuilles

### Tests
- [ ] Tests workflow inventaire
- [ ] Tests calcul écarts
- [ ] Tests E2E complet
```

### Phase 4 - Traçabilité

```markdown
## Sprint 6: Lots, Séries & Audit

### Backend
- [ ] Modèle Lot
- [ ] CRUD Lots
- [ ] Blocage/déblocage
- [ ] Gestion séries
- [ ] Traçabilité ascendante
- [ ] Traçabilité descendante
- [ ] Modèle AuditLog
- [ ] Middleware audit automatique
- [ ] Tests unitaires

### Frontend
- [ ] Liste/détail lots
- [ ] Blocage lot avec motif
- [ ] Vue traçabilité (arbre)
- [ ] Recherche séries
- [ ] Journal d'audit
- [ ] Filtres audit

### Tests
- [ ] Tests traçabilité
- [ ] Tests audit
- [ ] Tests E2E
```

### Phase 5 - Reporting

```markdown
## Sprint 7: Reporting & Dashboard

### Backend
- [ ] Service ValuationService
- [ ] Rapport état stocks
- [ ] Rapport valorisation
- [ ] Rapport mouvements
- [ ] Rapport écarts
- [ ] KPIs (rotation, couverture)
- [ ] Service AlertService
- [ ] Alertes stock bas/rupture
- [ ] Alertes péremption
- [ ] Export PDF/Excel
- [ ] Tests unitaires

### Frontend
- [ ] Dashboard principal
- [ ] Widgets KPI
- [ ] Rapport état stocks
- [ ] Rapport valorisation
- [ ] Graphiques mouvements
- [ ] Centre d'alertes
- [ ] Configuration alertes
- [ ] Export rapports

### Tests
- [ ] Tests calculs rapports
- [ ] Tests alertes
- [ ] Tests E2E dashboard
```

---

## INSTRUCTIONS FINALES POUR CLAUDE CODE

### À chaque développement de fonctionnalité :

1. **Lire les spécifications** de la fonctionnalité dans ce document
2. **Créer le modèle de données** selon les schémas définis
3. **Implémenter l'API** avec les endpoints spécifiés
4. **Ajouter les permissions** requises au middleware
5. **Créer les composants UI** selon la structure définie
6. **Écrire les tests** avant de passer à la suite
7. **Documenter** les endpoints dans API.md

### Règles strictes :

- **Ne jamais** créer de mouvement de stock sans passer par la machine à états
- **Toujours** vérifier les permissions avant chaque action
- **Toujours** logger dans l'audit les actions critiques
- **Ne jamais** modifier directement stock_levels sans passer par StockService
- **Toujours** valider les lots avant utilisation

### En cas de doute :

1. Consulter les modèles de données (section 3)
2. Vérifier les règles métier (section 6)
3. Suivre les workflows définis (section 8)
4. Respecter la matrice des permissions (section 5)

---

> **Ce skill est votre référence unique pour le développement du module de gestion de stock.**
> Suivez-le méthodiquement pour garantir la cohérence et la qualité du code produit.
