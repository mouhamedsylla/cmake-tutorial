# Guide Détaillé des Ressources CMake

## 1. Commandes Fondamentales

### add_executable()
**Syntaxe:**
```cmake
add_executable(<name> [WIN32] [MACOSX_BUNDLE]
               [EXCLUDE_FROM_ALL]
               [source1] [source2 ...])
```
**Utilisation:**
- Crée un exécutable à partir des fichiers sources
- Le premier argument est le nom de l'exécutable
- Les arguments suivants sont les fichiers sources

**Exemples:**
```cmake
# Simple
add_executable(monApp main.cpp)

# Multiple sources
add_executable(monApp 
    src/main.cpp
    src/utils.cpp
    src/math.cpp)

# Avec exclusion de la cible 'all'
add_executable(testApp test.cpp EXCLUDE_FROM_ALL)
```

### cmake_minimum_required()
**Syntaxe:**
```cmake
cmake_minimum_required(VERSION major.minor[.patch[.tweak]])
```
**Utilisation:**
- Définit la version minimale de CMake requise
- Doit être la première commande du CMakeLists.txt
- Configure les politiques CMake appropriées

**Exemples:**
```cmake
# Version simple
cmake_minimum_required(VERSION 3.10)

# Version spécifique avec patch
cmake_minimum_required(VERSION 3.10.2)
```

### project()
**Syntaxe:**
```cmake
project(<PROJECT-NAME>
        [VERSION <major>[.<minor>[.<patch>[.<tweak>]]]]
        [DESCRIPTION <project-description-string>]
        [HOMEPAGE_URL <url-string>]
        [LANGUAGES <language-name>...])
```
**Utilisation:**
- Définit le nom du projet et ses métadonnées
- Configure automatiquement plusieurs variables :
  - PROJECT_NAME
  - PROJECT_VERSION
  - PROJECT_SOURCE_DIR
  - PROJECT_BINARY_DIR

**Exemples:**
```cmake
# Simple
project(MonProjet)

# Complet
project(MonProjet
    VERSION 1.2.3
    DESCRIPTION "Description du projet"
    LANGUAGES CXX)
```

## 2. Variables de Configuration C++

### CMAKE_CXX_STANDARD
**Utilisation:**
- Définit le standard C++ à utiliser
- Valeurs possibles : 98, 11, 14, 17, 20, 23
- S'applique à toutes les cibles du projet

**Exemple:**
```cmake
set(CMAKE_CXX_STANDARD 17)
```

### CMAKE_CXX_STANDARD_REQUIRED
**Utilisation:**
- Force l'utilisation du standard spécifié
- Si ON, la compilation échoue si le standard n'est pas disponible
- Si OFF, utilise le standard disponible le plus proche

**Exemple:**
```cmake
set(CMAKE_CXX_STANDARD_REQUIRED ON)
```

## 3. Gestion des Versions

### <PROJECT-NAME>_VERSION_MAJOR/MINOR
**Utilisation:**
- Variables automatiquement définies par `project()`
- Accessibles après l'appel à `project(VERSION ...)`
- Utilisées pour le versionnage du projet

**Exemple:**
```cmake
project(MonProjet VERSION 1.2)
# MonProjet_VERSION_MAJOR = 1
# MonProjet_VERSION_MINOR = 2
```

## 4. Configuration et Inclusion

### configure_file()
**Syntaxe:**
```cmake
configure_file(<input> <output>
               [COPYONLY] [ESCAPE_QUOTES] [@ONLY]
               [NEWLINE_STYLE [UNIX|DOS|WIN32|LF|CRLF] ])
```
**Utilisation:**
- Copie un fichier en remplaçant les variables
- Utilise la syntaxe `@VAR@` ou `${VAR}` pour les variables
- Génère des fichiers de configuration

**Exemple:**
```cmake
# Config.h.in
#define VERSION_MAJOR @PROJECT_VERSION_MAJOR@
#define VERSION_MINOR @PROJECT_VERSION_MINOR@

# CMakeLists.txt
configure_file(
    "${PROJECT_SOURCE_DIR}/Config.h.in"
    "${PROJECT_BINARY_DIR}/Config.h"
)
```

### target_include_directories()
**Syntaxe:**
```cmake
target_include_directories(<target>
    <PRIVATE|PUBLIC|INTERFACE> [items1...]
    [<PRIVATE|PUBLIC|INTERFACE> [items2...] ...])
```
**Utilisation:**
- Spécifie les répertoires d'inclusion pour une cible
- Différents niveaux de visibilité :
  - PRIVATE : uniquement pour la cible
  - PUBLIC : pour la cible et ses dépendants
  - INTERFACE : uniquement pour les dépendants

**Exemple:**
```cmake
target_include_directories(monApp
    PUBLIC
        ${PROJECT_BINARY_DIR}
        include
    PRIVATE
        src
)
```

## Conseils d'Utilisation
1. **Organisation:**
   - Utilisez les commandes dans l'ordre logique
   - Groupez les configurations liées
   - Documentez les choix importants

2. **Bonnes Pratiques:**
   - Préférez `target_include_directories()` à `include_directories()`
   - Utilisez les niveaux de visibilité appropriés
   - Maintenez une structure de projet claire

3. **Débogage:**
   - Utilisez `message()` pour déboguer
   - Vérifiez les valeurs des variables avec `message(STATUS "Var = ${VAR}")`
   - Testez les configurations avec différentes versions de CMake
