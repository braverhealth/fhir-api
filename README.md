# FHIR Communication & Service Directory APIs - Documentation

## Accès à la Documentation

### Swagger UI (Documentation Interactive)

La documentation interactive des APIs est automatiquement déployée sur **GitHub Pages** à chaque push sur la branche `main`.

**URL** : https://braverhealth.github.io/fhir_comm/

---

## Structure du Projet

```
fhir_comm/
├── schema/                           # Définitions OpenAPI
│   ├── fhir-communication.yml        # API FHIR R4B Communication
│   └── fhir-service-directory.yml    # API pan-Canadian Care Service Directory (CA:CSD)
├── .github/
│   └── workflows/                    # GitHub Actions
│       ├── ci.yml                    # Validation des schémas OpenAPI
│       └── pages.yml                 # Déploiement Swagger UI sur GitHub Pages
└── index.html                        # Interface Swagger UI
```

---

## Vue d'ensemble des APIs

Ce dépôt contient deux définitions OpenAPI principales :

1. **`schema/fhir-communication.yml`** - API FHIR R4B Communication
2. **`schema/fhir-service-directory.yml`** - API pan-Canadian Care Service Directory (CA:CSD)

### 1. API FHIR Communication (`fhir-communication.yml`)

**But**: Gérer les ressources FHIR Communication selon la spécification HL7 FHIR R4B.

**Ressource principale**: `Communication` - Représente un message ou une notification échangée entre des parties dans un contexte de santé.

**Fonctionnalités**:
- Création, lecture, mise à jour et suppression de Communications
- Recherche avec filtres (patient, statut, catégorie, expéditeur, destinataire, etc.)
- Support du versionnement et de l'historique des ressources
- Payloads multiples (texte, pièces jointes, références)

**Endpoints**:
- `GET /Communication` - Recherche de Communications
- `POST /Communication` - Création d'une Communication
- `GET /Communication/{id}` - Lecture d'une Communication
- `PUT /Communication/{id}` - Mise à jour d'une Communication
- `DELETE /Communication/{id}` - Suppression d'une Communication
- `GET /Communication/{id}/_history` - Historique des versions

**Spécification de référence**: [HL7 FHIR R4B Communication](https://www.hl7.org/fhir/R4B/communication.html)

### 2. API Service Directory (`fhir-service-directory.yml`)

**But**: Implémenter le pan-Canadian Care Service Directory (CA:CSD), basé sur IHE mCSD (Mobile Care Services Discovery) v4.0.0.

**Ressources supportées**:
| Ressource | Description |
|-----------|-------------|
| `Organization` | Organisations administratives (hôpitaux, cliniques, HIEs) |
| `Location` | Lieux physiques (établissements, bâtiments, juridictions) |
| `Practitioner` | Professionnels de santé (médecins, infirmiers, pharmaciens) |
| `PractitionerRole` | Rôles des praticiens dans les organisations/lieux |
| `HealthcareService` | Services de santé offerts aux patients |
| `Endpoint` | Points d'accès électroniques pour l'échange de données |
| `OrganizationAffiliation` | Relations non-hiérarchiques entre organisations |

**Fonctionnalités**:
- Découverte de fournisseurs de soins à travers les juridictions
- Recherche géographique (par coordonnées et distance)
- Gestion des relations entre organisations, praticiens et services
- Support des endpoints d'interopérabilité (FHIR, IHE XCA/XDS, DICOM)

**Spécifications de référence**:
- [IHE mCSD v4.0.0](https://profiles.ihe.net/ITI/mCSD/)
- [Infoway CA:CSD](https://infoscribe.infoway-inforoute.ca/pages/viewpage.action?pageId=254444111)

---

## Modèle de Sécurité

Les deux APIs utilisent une authentification **JWT Bearer Token**.

```
Authorization: Bearer <JWT>
```

Chaque requête doit inclure un JWT valide dans l'en-tête `Authorization`.

---

## Développement Local

### Prérequis

- Node.js 22.x (voir `.tool-versions`)

### Visualiser la documentation

```bash
# Installer un serveur local
npm install -g serve

# Démarrer le serveur
serve .

# Ouvrir http://localhost:3000 dans le navigateur
```

### Valider les schémas OpenAPI

```bash
# Installer les outils de validation
npm install -g @redocly/cli

# Valider les schémas
redocly lint schema/fhir-communication.yml
redocly lint schema/fhir-service-directory.yml
```

---

## Spécifications FHIR

- **Version FHIR**: R4 (v4.0.1) / R4B (v4.3.0)
- **Content-Type**: `application/fhir+json`
- **Format des réponses**: JSON uniquement

### Types FHIR supportés

Les schémas incluent les types de données FHIR standard :
- `Identifier`, `CodeableConcept`, `Coding`
- `Reference`, `Period`, `Attachment`
- `HumanName`, `Address`, `ContactPoint`
- `Meta`, `Narrative`
- `Bundle` (pour les résultats de recherche)
- `OperationOutcome` (pour les erreurs)

