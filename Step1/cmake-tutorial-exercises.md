# Leçons des Exercices CMake

## Exercice 1 : Projet CMake de Base

### Points Clés
1. **Structure Minimale** : Un projet CMake basique nécessite seulement trois commandes :
   ```cmake
   cmake_minimum_required(VERSION 3.10)
   project(MonProjet)
   add_executable(Tutorial tutorial.cxx)
   ```

### Bonnes Pratiques
- Toujours commencer par `cmake_minimum_required()`
- Utiliser des commandes en minuscules (convention)
- Placer `project()` juste après `cmake_minimum_required()`
- Structure de build recommandée :
  ```bash
  mkdir build
  cd build
  cmake ..
  cmake --build .
  ```

## Exercice 2 : Gestion du Standard C++

### Points Clés
1. **Variables Spéciales CMake** :
   - Éviter de créer des variables commençant par `CMAKE_`
   - Configuration du standard C++ :
     ```cmake
     set(CMAKE_CXX_STANDARD 11)
     set(CMAKE_CXX_STANDARD_REQUIRED ON)
     ```

### Application Pratique
```cmake
# Exemple complet
cmake_minimum_required(VERSION 3.10)
project(Tutorial)

# Configurer le standard C++ avant add_executable
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(Tutorial tutorial.cxx)
```

## Exercice 3 : Gestion des Versions et Configuration des Headers

### Points Clés
1. **Configuration des Versions** :
   - Utiliser le système de version de `project()`
   - Les variables `<PROJECT-NAME>_VERSION_MAJOR/MINOR` sont créées automatiquement

2. **Headers Configurés** :
   - Utiliser `configure_file()` pour générer des headers
   - Système de substitution avec `@VAR@`
   - Nécessite de configurer les chemins d'inclusion

### Exemple Complet
```cmake
# CMakeLists.txt
cmake_minimum_required(VERSION 3.10)
project(Tutorial VERSION 1.0)

configure_file(
    TutorialConfig.h.in
    TutorialConfig.h
)

add_executable(Tutorial tutorial.cxx)

target_include_directories(Tutorial PUBLIC
    ${PROJECT_BINARY_DIR}
)
```

```cpp
// TutorialConfig.h.in
#define Tutorial_VERSION_MAJOR @Tutorial_VERSION_MAJOR@
#define Tutorial_VERSION_MINOR @Tutorial_VERSION_MINOR@
```

### Structure de Projet Recommandée
```
MonProjet/
├── CMakeLists.txt
├── tutorial.cxx
├── TutorialConfig.h.in
└── build/
    └── TutorialConfig.h (généré)
```

## Points Importants à Retenir

1. **Ordre des Opérations** :
   - Toujours définir la version minimale de CMake en premier
   - Configurer le projet et ses propriétés ensuite
   - Définir les standards et configurations avant de créer les cibles
   - Configurer les includes après la création des cibles

2. **Bonnes Pratiques de Configuration** :
   - Utiliser les variables de configuration pour éviter la duplication
   - Séparer la configuration de la logique de build
   - Maintenir une source unique de vérité pour les versions

3. **Gestion des Chemins** :
   - Utiliser `${PROJECT_BINARY_DIR}` pour les fichiers générés
   - Configurer correctement les chemins d'inclusion avec `target_include_directories()`
   - Séparer les includes publics et privés

4. **Organisation du Projet** :
   - Créer un dossier build séparé
   - Utiliser des fichiers de configuration (.in)
   - Structurer le projet de manière hiérarchique
