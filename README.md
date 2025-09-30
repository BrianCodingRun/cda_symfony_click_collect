# Click&Ship - Site E-commerce

Application e-commerce développée avec Symfony (backend) et React + Vite (frontend), intégrant le système de paiement Stripe.

## 🚀 Fonctionnalités

-   Authentification utilisateur (JWT)
-   Gestion de deux types d'utilisateurs : clients et vendeurs
-   Catalogue de produits avec catégories
-   Gestion du panier d'achat
-   Paiement sécurisé avec Stripe
-   Upload d'images produits
-   API REST avec API Platform

## 📋 Prérequis

-   PHP 8.1 ou supérieur
-   Composer
-   MySQL 5.7+ ou MariaDB 10.3+
-   Un compte Stripe (mode test pour le développement)

## 🛠️ Installation

### Backend (Symfony)

1. **Cloner le projet**

```bash
git clone cda_symfony_click_collect
cd cda_symfony_click_collect
```

2. **Installer les dépendances**

```bash
composer install
```

3. **Configurer les variables d'environnement**

Créer un fichier `.env.local` à la racine du projet :

```env
# Configuration de la base de données
DATABASE_URL="mysql://username:password@127.0.0.1:3306/clickship?serverVersion=8.0"

# Configuration Stripe
STRIPE_SECRET_KEY=sk_test_votre_cle_secrete_stripe

# Configuration JWT
JWT_SECRET_KEY=%kernel.project_dir%/config/jwt/private.pem
JWT_PUBLIC_KEY=%kernel.project_dir%/config/jwt/public.pem
JWT_PASSPHRASE=votre_passphrase

# URL du frontend pour les redirections Stripe
FRONTEND_URL=http://localhost:5173/

# Configuration CORS
CORS_ALLOW_ORIGIN='^https?://(localhost|127\.0\.0\.1)(:[0-9]+)?$'
```

4. **Créer la base de données**

```bash
php bin/console doctrine:database:create
```

5. **Exécuter les migrations**

```bash
php bin/console doctrine:migrations:migrate
```

6. **Générer les clés JWT**

```bash
php bin/console lexik:jwt:generate-keypair
```

7. **Configurer VichUploader**

Créer le dossier pour les uploads :

```bash
mkdir -p public/uploads/products
```

8. **Lancer le serveur de développement**

```bash
symfony server:start
```

Le backend est accessible sur `http://localhost:8000`

## 🔑 Configuration Stripe

1. Créer un compte sur [Stripe](https://stripe.com)
2. Récupérer vos clés API dans le Dashboard Stripe (section Développeurs > Clés API)
3. Utiliser les clés de **test** pour le développement (préfixées par `sk_test_` et `pk_test_`)
4. Configurer l'URL de webhook si nécessaire : `http://localhost:8000/api/stripe/webhook`

### Cartes de test Stripe

-   **Paiement réussi** : 4242 4242 4242 4242
-   **Paiement refusé** : 4000 0000 0000 0002
-   Date d'expiration : n'importe quelle date future
-   CVC : n'importe quel code à 3 chiffres

## 📊 Structure de la base de données

### Entités principales

-   **User** : Utilisateurs (clients et vendeurs)
-   **Product** : Produits mis en vente
-   **Category** : Catégories de produits
-   **Order** : Commandes passées
-   **OrderLine** : Lignes de commande (détail des produits commandés)

### Relations

-   Un User peut avoir plusieurs Products (vendeur)
-   Un User peut avoir plusieurs Orders (client)
-   Un Product appartient à une Category
-   Une Order contient plusieurs OrderLines
-   Une OrderLine référence un Product

## 🔒 Authentification

L'API utilise JWT (JSON Web Tokens) pour l'authentification.

### Inscription

**POST** `/api/register`

```json
{
    "email": "user@example.com",
    "password": "motdepasse",
    "isPro": false
}
```

### Connexion

**POST** `/api/login`

```json
{
    "email": "user@example.com",
    "password": "motdepasse"
}
```

Réponse :

```json
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGc...",
    "id": 1,
    "email": "user@example.com"
}
```

### Utilisation du token

Ajouter le token dans le header des requêtes authentifiées :

```
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGc...
```

## 🛍️ Endpoints principaux

### Produits

-   **GET** `/api/products` - Liste tous les produits
-   **GET** `/api/products/{id}` - Détail d'un produit
-   **POST** `/api/products` - Créer un produit (vendeur uniquement)

### Catégories

-   **GET** `/api/categories` - Liste toutes les catégories
-   **GET** `/api/categories/{id}` - Détail d'une catégorie

### Paiement Stripe

-   **POST** `/api/stripe/checkout-session` - Créer une session de paiement
-   **GET** `/api/stripe/success?session_id={id}` - Confirmation de paiement

### Commandes

-   **GET** `/api/orders` - Liste des commandes de l'utilisateur connecté
-   **GET** `/api/orders/{id}` - Détail d'une commande

## 📦 Technologies utilisées

### Backend

-   Symfony 6/7
-   Doctrine ORM
-   API Platform
-   LexikJWTAuthenticationBundle
-   VichUploaderBundle
-   Stripe PHP SDK

## 🧪 Tests

### Tester l'API avec Postman

1. Importer la collection Postman (si disponible)
2. S'inscrire via `/api/register`
3. Se connecter via `/api/login` pour obtenir un token
4. Utiliser le token pour les requêtes authentifiées

### Tester le paiement

1. Ajouter des produits au panier
2. Procéder au paiement
3. Utiliser une carte de test Stripe
4. Vérifier la création de la commande en base de données

## 🚀 Déploiement en production

### Backend

1. Passer en mode production dans `.env` :

```env
APP_ENV=prod
APP_DEBUG=0
```

2. Utiliser les clés Stripe de production
3. Configurer HTTPS obligatoire
4. Sécuriser les clés JWT
5. Configurer les CORS de manière restrictive
6. Optimiser l'autoloader :

```bash
composer install --no-dev --optimize-autoloader
```

7. Vider le cache :

```bash
php bin/console cache:clear
```

## 📝 Licence

Ce projet a été développé dans le cadre de la formation Concepteur Développeur d'Applications.

## 👤 Auteur

Brian COUPAMA - Formation CDA

## 📧 Support

Pour toute question, contactez votre formateur ou consultez la documentation officielle :

-   [Symfony](https://symfony.com/doc)
-   [API Platform](https://api-platform.com/docs)
-   [Stripe](https://stripe.com/docs)
-   [React](https://react.dev)
