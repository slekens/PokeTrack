# PokeTracker

A Pokédex-style iOS app built with SwiftUI as a reference project for modern iOS development practices.

## Overview

PokeTracker consumes the [PokéAPI](https://pokeapi.co) to display a paginated list of Pokémon with full detail pages, offline caching, favorites, and type-effectiveness lookups. The project is structured to demonstrate Clean Architecture, the Repository pattern, Use Cases, and Atomic Design in a real SwiftUI codebase.

## Features

| Feature | Description |
|---|---|
| Paginated list | Infinite scroll loading 20 Pokémon at a time |
| Search | Filter by name or Pokédex number |
| Detail view | Stats, moves, abilities, evolution chain, type effectiveness |
| Favorites | Mark Pokémon as favorites, filter list by favorites |
| Generation filter | Filter by generation (I–IX) and sort by number or name |
| List / Grid toggle | Switch between row and grid layout |
| Offline cache | SwiftData-backed cache for both list and detail data |
| Image cache | Two-level image cache (NSCache + FileManager) |

## Architecture

Clean Architecture with three layers inside the `Features/Pokemon` module:

```
Data ──► Domain ──► Presentation
```

| Layer | Contents |
|---|---|
| **Data** | DTOs, `PokemonRepository`, SwiftData persistence (`@Model` classes), `PokemonDataService` |
| **Domain** | Business models (`Pokemon`, `PokemonDetail`, `PokemonType`), Use Cases |
| **Presentation** | `@Observable` ViewModels, Views organized by Atomic Design |

`Core` holds utilities shared across features: `APIClient`, `Endpoint`, `NetworkError`, `ImageCache`, `ImageLoader`.

### Atomic Design in SwiftUI

Views are organized in three tiers:

| Tier | Examples |
|---|---|
| **Atoms** | `PokemonTypeTag`, `StatBar`, `FavoriteButton`, `PokemonIDLabel` |
| **Molecules** | `PokemonRowView`, `PokemonGridItem`, `PokemonStatsView`, `EvolutionChainView` |
| **Screens** | `PokemonListScreen`, `PokemonDetailScreen`, `FavoritesStatsScreen`, `FilterScreen` |

## Project Structure

```
PokeTracker/
├── PokeTrackerApp.swift
├── Core/
│   ├── Cache/
│   │   ├── ImageCache.swift          # NSCache + FileManager two-level cache
│   │   └── ImageLoader.swift         # Actor-based image loader
│   ├── Network/
│   │   ├── APIClient.swift           # async/await HTTP client
│   │   ├── Endpoint.swift            # PokeAPI endpoint definitions
│   │   └── NetworkError.swift        # Typed network errors
│   └── Extensions/
│       └── URL+Extensions.swift
└── Features/Pokemon/
    ├── Data/
    │   ├── DTOs/
    │   │   ├── PokemonListDTO.swift
    │   │   ├── PokemonDetailDTO.swift
    │   │   ├── PokemonSpeciesDTO.swift
    │   │   └── EvolutionChainDTO.swift
    │   ├── Persistence/
    │   │   ├── CachedPokemon.swift        # @Model
    │   │   ├── CachedPokemonDetail.swift  # @Model
    │   │   ├── FavoritePokemon.swift      # @Model
    │   │   └── PokemonDataService.swift   # SwiftData CRUD
    │   └── Repository/
    │       └── PokemonRepository.swift
    ├── Domain/
    │   ├── Models/
    │   │   ├── Pokemon.swift
    │   │   ├── PokemonDetail.swift
    │   │   ├── PokemonMove.swift
    │   │   ├── PokemonType.swift          # Type colors + effectiveness
    │   │   ├── TypeEffectiveness.swift
    │   │   └── Evolution.swift
    │   └── UseCases/
    │       ├── GetPokemonListUseCase.swift
    │       └── GetPokemonDetailUseCase.swift
    └── Presentation/
        ├── ViewModels/
        │   ├── PokemonListViewModel.swift  # @Observable, pagination, filters
        │   └── PokemonDetailViewModel.swift
        └── Views/
            ├── Atoms/
            ├── Molecules/
            └── Screens/

PokeTrackerTests/
├── Mocks/
│   ├── MockAPIClient.swift
│   ├── MockPokemonRepository.swift
│   └── TestData.swift
├── EndpointTests.swift
├── PokemonRepositoryTests.swift
├── GetPokemonListUseCaseTests.swift
├── GetPokemonDetailUseCaseTests.swift
└── PokemonTypeTests.swift
```

## Tech Stack

- **SwiftUI** — declarative UI
- **SwiftData** — offline persistence (`@Model`, `ModelContainer`, `ModelContext`)
- **Swift Concurrency** — `async/await`, `actor`, `@MainActor`
- **Observation** — `@Observable` ViewModels (Swift 5.9+)
- **Swift Testing** — unit test suite (`@Suite`, `@Test`, `#expect`)
- **PokeAPI** — free, open Pokémon REST API

## Requirements

| | Minimum |
|---|---|
| iOS | 17.0 |
| Xcode | 16.0 |
| Swift | 5.9 |

## Getting Started

1. Clone the repository
2. Open `PokeTracker.xcodeproj` in Xcode
3. Select a simulator running iOS 17 or later
4. Build and run (`⌘R`) — no API keys or additional setup required

## API

All data comes from [PokéAPI v2](https://pokeapi.co/docs/v2). No authentication required.

```
GET https://pokeapi.co/api/v2/pokemon?limit=20&offset=0
GET https://pokeapi.co/api/v2/pokemon/{id}
GET https://pokeapi.co/api/v2/pokemon-species/{id}
GET https://pokeapi.co/api/v2/evolution-chain/{id}
```

## Author

**Abraham Abreu** — [slekens.dev](https://slekens.dev)

## License

For educational purposes.
