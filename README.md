# OpenStream

Une interface de streaming moderne, open source, construite avec React, TypeScript, Vite, Tailwind CSS et Framer Motion.

OpenStream est une **interface**. Elle ne contient, n'héberge et ne distribue aucun contenu protégé par copyright : toutes les métadonnées et sources vidéo passent par l'abstraction `MediaProvider`, à connecter à une API légale (catalogue sous licence) ou à des sources fournies par l'utilisateur (fichiers auto-hébergés, serveur média personnel, etc.).

## Démarrer

```bash
npm install
npm run dev
```

L'application démarre avec un provider de démonstration (`MockMediaProvider`) : un catalogue fictif avec des affiches de substitution et un unique clip vidéo du domaine public, pour que toutes les pages soient explorables sans clé d'API.

## Connecter une vraie source de données

1. Implémentez l'interface `MediaProvider` (`src/providers/MediaProvider.ts`) contre votre API de métadonnées (compatible TMDB, catalogue sous licence, etc.).
2. Renseignez `VITE_API_URL` dans `.env` (voir `.env.example`). **Ne mettez jamais de clé API dans une variable préfixée `VITE_`** — elle serait exposée au navigateur. Gardez les clés côté serveur (backend Express, edge function...).
3. Remplacez l'export dans `src/services/providers/index.ts` par votre implémentation.

Tout le reste de l'application (pages, hooks, composants) ne dépend que de l'interface `MediaProvider` : changer de backend ne touche jamais à l'UI.

## Structure

```text
src/
├── components/       # Navbar, MovieCard, VideoPlayer, etc.
│   ├── layout/
│   ├── media/
│   ├── player/
│   └── ui/
├── pages/            # Home, Search, MovieDetail, SeriesDetail, Watch, MyList, History
├── layouts/          # MainLayout (nav + transitions), PlayerLayout (plein écran)
├── hooks/            # useAsync, useDebounce, useMyList, useHistory
├── services/
│   ├── providers/    # point d'entrée — swap le provider ici
│   └── mock/         # données et provider de démonstration
├── providers/         # interface MediaProvider
├── types/             # types partagés (Movie, Series, Episode, VideoSource...)
├── stores/            # Ma liste / historique, persistés en localStorage
└── utils/
```

## Fonctionnalités

- Accueil avec hero, carrousels (tendances, populaires, nouveautés, mieux notés, continuer à regarder, ma liste)
- Recherche instantanée avec filtres (type, genre, année, note) et chargement progressif
- Pages film et série détaillées, avec casting et contenus similaires
- Sélecteur de saisons/épisodes
- Lecteur vidéo complet : lecture/pause, volume, plein écran, picture-in-picture, sélection de qualité/source, sous-titres, raccourcis clavier, lecture automatique de l'épisode suivant
- Ma liste et historique de visionnage, persistés en `localStorage`
- Skeletons de chargement, gestion d'erreurs, états vides partout

## Important

Le lecteur n'implémente et ne contourne aucun système de DRM. `getSources()` doit uniquement renvoyer des sources que l'opérateur est légalement autorisé à servir.
