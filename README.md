# Gym Tracker

Application web de suivi d'entraînement personnel, déployée sur Azure Web App.

## Fonctionnement général

L'application est un fichier HTML unique (`index.html`) qui gère l'intégralité de la logique côté client. Elle utilise **Supabase** comme backend (authentification + base de données) et l'**API Anthropic (Claude)** pour la génération automatique de programmes.

## Fonctionnalités

### Authentification
- Connexion par email/mot de passe via Supabase Auth
- Gestion de session persistante (token stocké dans `localStorage`)
- Réinitialisation de mot de passe par email
- Flux d'inscription pour nouveaux utilisateurs (sur invitation)

### Gestion des programmes
- Création de programmes d'entraînement via un assistant pas-à-pas (wizard)
- **Génération par IA** : l'application envoie les paramètres (objectif, niveau, équipement, jours disponibles) à Claude via l'API Anthropic pour générer un programme personnalisé
- Modification du nom et de l'icône d'un programme existant
- Suppression de programmes

### Suivi des séances
- Vue hebdomadaire avec navigation entre les semaines
- Planification des jours de repos / jours d'entraînement
- Grille d'exercices par jour avec groupes musculaires ciblés
- Enregistrement des séries, répétitions et charges pour chaque exercice
- Sauvegarde des données dans Supabase (table dédiée par utilisateur)

### Modal d'exercice
- Détail d'un exercice : séries, répétitions, temps de repos
- **Conseils IA** : génération de conseils personnalisés via l'API Claude
- Graphique de progression de la charge au fil des semaines (Chart.js)
- Saisie et sauvegarde des performances (poids, machine utilisée, notes)

### Profil utilisateur
- Modification du prénom, nom et email
- Changement de mot de passe

### Panneau d'administration
- Gestion des utilisateurs actifs (ajout, suppression)
- Gestion des invitations en attente
- Configuration de la clé API Anthropic (stockée dans Supabase et synchronisée entre appareils)

## Stack technique

| Composant | Technologie |
|---|---|
| Frontend | HTML / CSS / JavaScript vanilla (SPA mono-fichier) |
| Base de données & Auth | Supabase (PostgreSQL + Auth) |
| Génération IA | API Anthropic (Claude) |
| Graphiques | Chart.js 4.4 |
| Hébergement | Azure Web App |
| CI/CD | GitHub Actions (`main_entrainement.yml`) |

## Déploiement

Le déploiement est automatique à chaque push sur la branche `main` via le workflow GitHub Actions `.github/workflows/main_entrainement.yml` :

1. **Build** : installation des dépendances Node.js et build
2. **Deploy** : déploiement sur l'Azure Web App `entrainement` (slot Production) via OIDC

## Configuration requise

Pour utiliser l'application, les éléments suivants doivent être configurés :

- **Supabase** : URL et clé publique (`SUPABASE_URL`, `SUPABASE_KEY`) ainsi qu'une clé service pour l'administration
- **Clé API Anthropic** : saisie par l'administrateur depuis le panneau d'administration, stockée dans Supabase et synchronisée sur tous les appareils
