# 📚 Cours Complet : Nuxt 4 pour Développeurs Vue.js Avancés

Ce cours est conçu pour les étudiants en informatique ayant déjà de **solides bases en Vue.js et JavaScript**. Nous explorons **Nuxt 4**, le méta-framework qui étend Vue.js pour le développement d'applications universelles (SSR), performantes et full-stack.

## 🗺️ Sommaire du Cours

| Chapitre | Thème Principal |
| :--- | :--- |
| **1** | **Introduction et Mise en Place de Nuxt 4** |
| **2** | **Routage et Navigation Avancée** |
| **3** | **Gestion des Données (Data Fetching)** |
| **4** | **Le Rendu (Rendering) et le Déploiement** |
| **5** | **Architecture Full-Stack avec Nitro** |
| **6** | **Modules et Écosystème Nuxt** |
| **🚀** | **Mini-Projet Final : Catalogue de Produits Dynamique** |

-----

## 📝 Chapitre 1 : Introduction et Mise en Place de Nuxt 4

### Description

Ce chapitre pose les fondations. Nous explorerons pourquoi Nuxt est le choix privilégié pour les applications Vue.js de production (performance, SEO, DX). Nous installerons et configurerons un projet Nuxt 4, en insistant sur la **structure des dossiers basée sur la convention** et la configuration de base via `nuxt.config.ts`.

### Concepts Essentiels 💡

  * **Structure par Convention :** Nuxt utilise des dossiers spéciaux. Leur présence dicte le comportement de l'application.
      * *Exemple :* Le dossier **`pages/`** est toujours utilisé pour les routes, et **`components/`** pour les composants réutilisables.
  * **Auto-Importation des Composants :** Les composants placés dans le dossier `components/` sont automatiquement disponibles sans nécessiter d'instruction `import`.
      * *Exemple :*
        1.  Créez `components/AppLogo.vue`.
        2.  Dans `app.vue` (ou n'importe quelle autre page/composant), vous pouvez l'utiliser directement :
        <!-- end list -->
        ```html
        <template>
          <div>
            <AppLogo /> <h1>Bienvenue sur Nuxt !</h1>
            <NuxtPage />
          </div>
        </template>
        ```
  * **`nuxt.config.ts` :** Le fichier de configuration principal.
      * *Exemple de Configuration :* Pour définir un titre global et un préfixe de route.
        ```typescript
        // nuxt.config.ts
        export default defineNuxtConfig({
          app: {
            head: {
              title: 'Mon Application Nuxt Ultime' // Le titre global
            }
          },
          router: {
            options: {
              // Toutes les routes commenceront par /app
              prefix: '/app' 
            }
          }
        })
        ```

### Exercices

1.  **Initialisation :** Créez un nouveau projet Nuxt 4.
2.  **Configuration :** Modifiez le fichier **`nuxt.config.ts`** pour définir le titre global de l'application et ajouter un préfixe de route global (`/app`).
3.  **Composant Auto-Importé :** Créez un composant simple (`<AppLogo>`) dans le dossier **`components/`** et utilisez-le dans **`app.vue`** **sans l'importer manuellement**.

-----

## 📝 Chapitre 2 : Routage et Navigation Avancée

### Description

Nuxt gère le routage par les fichiers. Nous verrons les **routes statiques et dynamiques**, les **layouts** pour les éléments récurrents, et les **middlewares** pour contrôler l'accès aux pages.

### Concepts Essentiels 💡

  * **Routes Dynamiques :** Utiliser des crochets `[]` pour capturer des segments d'URL.
      * *Exemple :* `pages/products/[id].vue`
        ```html
        <script setup lang="ts">
        const route = useRoute()
        const productId = route.params.id
        </script>

        <template>
          <h1>Détail du Produit n°: {{ productId }}</h1>
          <NuxtLink to="/products/2">Voir Produit 2</NuxtLink> 
        </template>
        ```
  * **Layouts :** Structures d'interface réutilisables définies dans `layouts/`. Le contenu de la page est injecté grâce à **`<slot />`** dans le layout.
      * *Exemple d'Application :*
        ```html
        <script setup lang="ts">
        // Applique le layout 'admin.vue' (doit exister dans layouts/admin.vue)
        definePageMeta({
          layout: 'admin' 
        })
        </script>

        <template>
          <h2>Tableau de Bord Administrateur</h2>
        </template>
        ```
  * **Middlewares :** Fonctions exécutées avant le chargement d'une page (dans `middleware/`). Ils utilisent **`MapsTo()`** pour la redirection.
      * *Exemple de Middleware :*
        ```typescript
        // middleware/auth.ts
        export default defineNuxtRouteMiddleware((to, from) => {
          const isAuthenticated = false // Simule un utilisateur non authentifié
          
          if (!isAuthenticated) {
            return navigateTo('/') // Redirection vers la page d'accueil
          }
        })
        ```

### Exercices

1.  **Routage Dynamique :** Créez une page **`pages/products/[id].vue`** qui affiche l'ID du produit extrait du paramètre de la route.
2.  **Layouts :** Créez un **`layouts/default.vue`** avec un `<header>` et le composant **`<NuxtPage />`**. Créez un **`layouts/admin.vue`** simple et appliquez-le à une page spécifique **`pages/admin/index.vue`** en utilisant `definePageMeta`.
3.  **Middleware :** Créez un middleware **`middleware/auth.ts`** qui vérifie une condition simple et redirige l'utilisateur vers la page d'accueil si la condition n'est pas remplie. Appliquez ce middleware à votre page `admin/index.vue`.

-----

## 📝 Chapitre 3 : Gestion des Données (Data Fetching)

### Description

Ce chapitre est crucial pour la performance. Nous allons explorer les **composables de *data fetching*** de Nuxt, permettant de charger des données en bénéficiant du **Server-Side Rendering (SSR)**.

### Concepts Essentiels 💡

  * **`$fetch` :** L'outil de *data fetching* universel, basé sur *ofetch*. Il fonctionne côté serveur et client.
  * **`useFetch` :** Le composable clé pour le chargement des données, bénéficiant du SSR et de l'hydratation.
      * *Propriétés retournées :* **`data`**, **`pending`** (chargement), **`error`**, **`refresh`** (rechargement manuel).
      * *Exemple :*
        ```html
        <script setup lang="ts">
        // Le chargement se fait côté serveur et est hydraté côté client.
        const { data: users, pending, error, refresh } = await useFetch('https://jsonplaceholder.typicode.com/users')
        </script>

        <template>
          <button @click="refresh()">Recharger</button>
          <div v-if="pending">Chargement...</div>
          <ul v-else>
            <li v-for="user in users" :key="user.id">{{ user.name }}</li>
          </ul>
        </template>
        ```
  * **`useAsyncData` :** Utilisé pour récupérer des données à partir d'une fonction asynchrone arbitraire (pas seulement un appel API) avec les mêmes bénéfices SSR.
  * **`onMounted` et `Client-Side Only` :** Utiliser `onMounted` garantit que le code s'exécute uniquement après l'hydratation côté client, obligatoire pour le code qui dépend des APIs du navigateur (`window`, `localStorage`).

### Exercices

1.  **`useFetch` de base :** Dans une page, utilisez **`useFetch('https://jsonplaceholder.typicode.com/users/1')`** pour afficher les données d'un utilisateur. Affichez un état de chargement (**`pending.value`**) et une gestion des erreurs (**`error.value`**).
2.  **Rafraîchissement manuel :** Utilisez **`useFetch`** pour charger une liste d'éléments. Ajoutez un bouton pour appeler la fonction **`refresh()`** du composable afin de recharger les données côté client.
3.  **Appel côté client (Client-Side) :** Effectuez un appel **`$fetch`** qui ne doit se produire **que côté client** (dans le hook **`onMounted`**) et expliquez pourquoi cette approche est parfois nécessaire.

-----

## 📝 Chapitre 4 : Le Rendu (Rendering) et le Déploiement

### Description

Nous étudierons les différents **modes de rendu** (SSR, CSR, SSG, Hybrid) et comment les configurer pour optimiser la performance et le SEO.

### Concepts Essentiels 💡

  * **Server-Side Rendering (SSR) :** La page HTML est générée sur le serveur à chaque requête. (Mode par défaut de Nuxt).
  * **Static Site Generation (SSG) :** Les pages sont pré-rendues en HTML statique **au moment du *build***.
      * *Configuration :* Utilisation de la clé `prerender` dans `nuxt.config.ts`.
        ```typescript
        // nuxt.config.ts
        export default defineNuxtConfig({
          nitro: {
            prerender: {
              // Liste des routes à générer en statique au build
              routes: ['/about', '/'] 
            }
          }
        })
        ```
  * **Client-Side Rendering (CSR) :** Rendu entièrement côté client.
  * **`<ClientOnly>` :** Composant intégré pour forcer le rendu de son contenu **uniquement côté client** après l'hydratation.
      * *Exemple :*
        ```html
        <ClientOnly fallback="Chargement de l'API de géolocalisation...">
          <MapComponent /> 
        </ClientOnly>
        ```

### Exercices

1.  **Configuration SSG :** Configurez votre **`nuxt.config.ts`** pour pré-rendre une page spécifique (ex: `/about`) en mode SSG en utilisant l'option `prerender.routes`.
2.  **Rapport de *Build* :** Lancez la commande de *build* (`npm run build`) et analysez le dossier **`.output/`**. Identifiez les fichiers générés pour le serveur (Nitro) et ceux pour le client (statiques).
3.  **Force CSR :** Dans un composant spécifique, utilisez le composant **`<ClientOnly>`** pour forcer le rendu de ce composant côté client, et expliquez les cas d'usage où cela est pertinent.

-----

## 📝 Chapitre 5 : Architecture Full-Stack avec Nitro

### Description

**Nitro** est le moteur de serveur de Nuxt, permettant de créer une **API Server** directement dans votre projet. Nous apprendrons à créer des **endpoints d'API**, des **routes de serveur** et des **plugins de serveur**.

### Concepts Essentiels 💡

  * **Endpoints d'API (`server/api/`) :** Les fichiers placés ici deviennent des endpoints RESTful accessibles via `/api/...`.
      * *Exemple d'Endpoint :* `server/api/hello.ts`
        ```typescript
        // server/api/hello.ts
        import { defineEventHandler } from 'h3' // Utility from Nitro's core

        export default defineEventHandler(async (event) => {
          return { 
            message: "Bonjour de Nitro!",
            method: event.method // Accès aux propriétés de la requête
          }
        })
        ```
  * **Routes de Serveur (`server/routes/`) :** Destinées aux requêtes complexes ou aux fonctions de serveur.
      * *Récupération des paramètres d'URL :* Utilisez la fonction `getRouterParam(event, 'paramName')`.
  * **Plugins de Serveur (`server/plugins/`) :** Code qui s'exécute **une seule fois** au démarrage du serveur. Parfait pour l'initialisation.

### Exercices

1.  **Endpoint API :** Créez un endpoint dans **`server/api/hello.ts`** qui retourne simplement un objet JSON. Utilisez **`$fetch('/api/hello')`** dans une page pour afficher le message.
2.  **Route de Serveur avec Paramètre :** Créez une route de serveur dynamique **`server/routes/users/[id].ts`** qui lit un paramètre d'URL (l'ID) en utilisant `getRouterParam(event, 'id')` et le retourne dans la réponse.
3.  **Plugin de Serveur :** Créez un plugin de serveur simple dans **`server/plugins/log.ts`** qui utilise le hook `nitroApp.hooks.hook('request', ...)` pour loguer chaque requête reçue par le serveur.

-----

## 📝 Chapitre 6 : Modules et Écosystème Nuxt

### Description

L'écosystème de **modules** est la force de Nuxt, permettant d'ajouter des fonctionnalités complexes (gestion d'état, UI, CMS) avec très peu de configuration. Nous apprendrons à utiliser ces modules et à comprendre comment ils étendent les fonctionnalités de Nuxt.

### Concepts Essentiels 💡

  * **Modules :** Des fonctions qui étendent le cœur de Nuxt en ajoutant de nouvelles fonctionnalités. Installés via npm et ajoutés à la clé **`modules`** dans `nuxt.config.ts`.
      * *Exemple d'Installation :*
        ```bash
        npm install @nuxt/ui
        ```
      * *Exemple de Configuration :*
        ```typescript
        // nuxt.config.ts
        export default defineNuxtConfig({
          modules: ['@nuxt/ui'], // Ajout du module
          ui: { // Configuration spécifique du module
            global: true, 
            icons: ['mdi']
          }
        })
        ```
  * **Hooks Nuxt :** Les modules utilisent des *hooks* (points d'entrée) comme `ready` ou `app:resolve` pour injecter du code, des composants ou des plugins dans le processus de *build* de Nuxt.

### Exercices

1.  **Installation de Module :** Installez le module **`@nuxt/ui`** (ou un autre module d'interface utilisateur/contenu de votre choix).
2.  **Configuration de Module :** Utilisez le module installé dans une page et configurez une de ses options dans **`nuxt.config.ts`**.
3.  **Création de Module Simple (Bonus) :** Créez un module local simple qui utilise la fonction `addImports` (via `@nuxt/kit`) pour rendre un **composable de votre choix** disponible automatiquement dans tout le projet.

-----

## 🚀 Mini-Projet Final : Catalogue de Produits Dynamique

Ce projet doit mobiliser les connaissances acquises dans les chapitres 2, 3 et 5.

### Cahier des Charges

1.  **Initialisation :** Projet Nuxt 4 vierge.
2.  **API Serveur (Nitro) :**
      * Créez un endpoint **`GET /api/products`** qui retourne un tableau d'objets `Product` simulés (ID, nom, prix, description).
      * Créez un endpoint **`GET /api/products/[id]`** qui retourne un produit spécifique basé sur son ID, ou une erreur 404 (utilisez `createError` de Nitro).
3.  **Interface Utilisateur (Pages) :**
      * **Page d'Accueil (`/`) :** Affiche une liste de tous les produits. Le *fetching* des données doit utiliser **`useFetch`** pour bénéficier du **SSR**. Chaque produit est un lien vers sa page de détail.
      * **Page de Détail (`/products/[id]`) :** Affiche les détails du produit correspondant. Utilisez **`useFetch`** pour récupérer le produit spécifique en se basant sur le paramètre d'URL. Gérez l'état 404 (produit non trouvé) avec l'utilitaire `showError` de Nuxt.
4.  **Mise en Page :** Utilisez un **Layout** global qui inclut une barre de navigation simple et un pied de page.
5.  **Middleware (Bonus) :** Ajoutez un **middleware** sur la page de détail pour vérifier que l'ID du produit est un nombre valide avant de tenter l'appel à l'API.

### Critères de Notation

| Critère | Poids | Description |
| :--- | :--- | :--- |
| **API Serveur (Nitro)** | 25% | Respect des endpoints, gestion des paramètres et de la logique de 404. |
| **Data Fetching (SSR)** | 30% | Utilisation correcte et performante de `useFetch` (données chargées côté serveur, gestion des états `pending`/`error`). |
| **Routage et Layouts** | 20% | Routage dynamique fonctionnel, utilisation d'un Layout et navigation cohérente. |
| **Conventions et DX** | 15% | Respect de la structure Nuxt (auto-imports, structure des dossiers), code clair et typé (TypeScript). |
| **Middleware/Gestion d'Erreur** | 10% | Implémentation du middleware (bonus) et gestion élégante de l'erreur 404. |

-----

Si vous souhaitez commencer le projet, je peux vous aider à initialiser le squelette de l'application \!
