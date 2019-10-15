# Hierarchical Front-End Structure

![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg) ![License: CC-BY-4.0](https://img.shields.io/badge/license-CC%20BY%204.0-brightgreen.svg) [![likeadev](https://img.shields.io/twitter/follow/likeadev?style=social)](https://twitter.com/likeadev)

Maintainable, framework-agnostic, hierarchical structure proposal with comprehensive guide for every front-end project out there. If you're asking yourself how to structure your project - look no further.

## Table of Contents

1. **[Introduction](#introduction)**
2. **[Project Structure](#project-structure)**
3. **[Guidelines](#guidelines)**

    2.1. **[Separation of concerns](#user-content-1-separation-of-concerns)**

    2.2. **[Directory structure](#user-content-2-directory-structure)**

    2.3. **[Hierarchy](#user-content-3-hierarchy)**

    2.4. **[File and directory naming](#user-content-4-file-and-directory-naming)**

4. **[Example of Implementation](#example-of-implementation)**
5. **[Contribution](#contribution)**
6. **[License](#license)**

## Introduction

Hierarchical Front-End Structure is based on a principle of **making directories (modules) as self-contained as possible and organized in a cascading hierarchy**.

The concept is similar to a predictable story divided into chapters. Read with minimal effort and having content easily consumable by the reader. It's consistent in it's plot, without any twists. Finding anything in the code should be as simple as opening a page and quickly traversing through it.

That's what this proposal enables you to have - a readable, predictable and consistent structure. It will allow you to actually do things without wasting time trying to understand potentially tangled project.

## Project Structure

The structure below is a proposal of the implementation of a project that follows the [Guidelines](#guidelines) section.

```bash
├── jest # "Jest/any other testing framework" setup files and utility scripts
│   ├── setupAfterEnv.js
│   └── ...
├── src
│   ├── assets # Global/shared asset files
│   │   └── styles
│   │       ├── fonts.sass
│   │       ├── mixins.sass
│   │       └── ...
│   ├── api # Endpoint communication
│   │   ├── entries.js
│   │   ├── users.js
│   │   └── ...
│   ├── components
│   │   ├── layout # Non-route specific components used as basic, re-usable building elements of the app
│   │   │   ├── Layout
│   │   │   │   ├── index.jsx
│   │   │   │   ├── styles.sass
│   │   │   │   └── ...
│   │   │   ├── Form
│   │   │   │   ├── index.jsx
│   │   │   │   └── ...
│   │   │   ├── Input
│   │   │   │   ├── index.jsx
│   │   │   │   ├── styles.sass
│   │   │   │   └── ...
│   │   │   ├── Loader
│   │   │   │   ├── index.jsx
│   │   │   │   ├── styles.sass
│   │   │   │   └── ...
│   │   │   └── ...
│   │   ├── other # Utility components that are not displayed directly to the user but perform their functions in the background
│   │   │   ├── Route
│   │   │   │   └── index.jsx
│   │   │   └── ...
│   │   ├── pages # Components dedicated to specific routes / pages
│   │   │   ├── Main # "/main" route
│   │   │   │   ├── Entry
│   │   │   │   │   ├── index.jsx
│   │   │   │   │   ├── styles.sass
│   │   │   │   │   └── ...
│   │   │   │   ├── index.jsx
│   │   │   │   ├── test.jsx
│   │   │   │   └── ...
│   │   │   └── SignIn # "/sign-in"
│   │   │       ├── Content
│   │   │       │   ├── index.jsx
│   │   │       │   ├── styles.sass
│   │   │       │   └── ...
│   │   │       ├── index.jsx
│   │   │       ├── test.jsx
│   │   │       └── ...
│   │   └── shared # Wrappers for "node_modules" components. If you want to wrap an external component / function served from a framework like Material-UI - this is where you put it
│   │       ├── Container
│   │       │   ├── index.jsx
│   │       │   ├── styles.sass
│   │       │   └── ...
│   │       ├── Icon
│   │       │   ├── index.jsx
│   │       │   ├── styles.sass
│   │       │   └── ...
│   │       ├── index.js # Exports wrappers with non-wrapped components of external framework. Acts as single source of component imports for the specific framework
│   │       └── ...
│   ├── jest # "Jest/any other testing framework" utilities used specifically for testing purposes inside tests, e.g. helper functions
│   │   ├── utils
│   │   │   ├── axiosMock.js
│   │   │   └── ...
│   │   └── ...
│   ├── routing # Routes and routing related code
│   │   ├── routes.js
│   │   └── ...
│   ├── store # Global store files, e.g. Redux, Vuex
│   │   └── user
│   │   │   ├── actions.js
│   │   │   ├── reducers.js
│   │   │   └── ...
│   │   ├── index.jsx
│   │   └── ...
│   ├── utils
│   │   ├── localStorage.js
│   │   ├── userStorage.js
│   │   ├── theme.js
│   │   └── ...
│   ├── index.html # Main html file
│   ├── index.jsx # Application main entry script
│   └── ...
├── babel.config.js # Root directory containing project-related files
├── jest.config.js
├── package.json
├── webpack.config.js
└── ...
```

A word of note:

- All directories can be adjusted to your needs with respect to the [Guidelines](#guidelines). This will make your cooperation with fellow developers smooth and able to direct them to this file for reference
- You may not necessarily use global store, routing or any other part of the above-mentioned example, so it's recommended to skip on any of these or to replace them if needed

## Guidelines

Follow these guidelines when implementing structure presented in [Project Structure](#project-structure) section:

### 1. Separation of concerns

1. Each file should address one concern. Don't create "jack-of-all-trades" files, e.g. a file that contains the component implementation, styles, tests and utility functions.
2. Don't overgranulate. One file to store certain concern is enough, no need to split it per function/variable it contains, e.g. file with constants representing user types, file with functions allowing user storage manipulation.

#### ❌ Bad

```bash
└── src
    └── components
        └── pages
            └── Main
                └── indexAllInOne.js  # Contains all parts of the `Main` component - the component itself, styles, tests and utility functions
```

#### ✅ Good

```bash
└── src
    └── components
        └── pages
            └── Main
                ├── utils
                │   └── findUser.js
                ├── index.js
                ├── test.js
                └── style.sass
```

### 2. Directory structure

Directories are module-like entities. These modules should be self-contained. Meaning that if a file would be moved it should be relocated with all it's exclusive dependencies. The process should be as easy as drag and drop is. To achieve this, follow these rules:

1. Group the files in directories that describe their role, e.g. `src/constants`, `src/components/utils`, `src/components/pages/Main`
2. Files nested in directories should be in direct relation with their parent, e.g. `src/components/pages/Main/List`
3. Create a separate directory for the file if there is at least one file/directory in direct relation with it, e.g. `src/store`

#### ❌ Bad

```bash
├── src
│   ├── storeArticles # `storeArticles` is in direct relation with `store`
│   │   ├── actions.ts
│   │   └── reducers.ts
│   ├── store
│   │   └── index.js
│   └── components
│       └── pages
│           ├── MainList # `MainList` is child of the `Main` component
│           │   └── index.js
│           └── Main
│               ├── utils
│               │   ├── findUser.js
│               │   └── findUserTest.js # `findUserTest.js` is in direct relation with `findUser.js`
│               └── index.js
└── styles
    └── mainList.sass # contains styles specific to `MainList` component and is in direct relation with this component's index file
```

#### ✅ Good

```bash
└── src
    ├── store
    │   ├── articles
    │   │   ├── actions.ts
    │   │   └── reducers.ts
    │   └── index.js
    └── components
        └── pages
            └── Main
                ├── utils
                │   └── findUser
                │       ├── index.js
                │       └── test.js
                ├── List
                │   ├── index.js
                │   └── style.sass
                └── index.js
```

### 3. Hierarchy

> This section is strongly tied to [Directory structure](#directory-structure) but focuses specifically on hierarchy of related files

Code should be put in files that are organized in a cascading hierarchy.

1. If a file is used only in one place it should be stored in the directory it's being used in
2. If a shared piece of code is not in sibling or higher-level directory that files it's being used in are - it should be relocated to the same level or higher

#### ❌ Bad

```bash
└── src
    └── components
        └── pages
            └── Main
                ├── List
                │   └── index.js
                └── Chart
                    ├── utils
                    │   └── findUser.js # `findUser.js` is used by both `List` and `Chart` components
                    └── index.js
```

#### ✅ Good

```bash
└── src
    └── components
        └── pages
            └── Main
                ├── utils
                │   └── findUser.js # `findUser.js` is used by both `List` and `Chart` components
                ├── List
                │   └── index.js
                └── Chart
                    └── index.js
```

`findUser.js` has been moved up one level in the hierarchy. This indicates to the reader that `findUser.js` is used in at least one of the sibling directories or files - `List` and/or `Chart`.

---

#### ❌ Bad

```bash
└── src
    └── components
        └── pages
            ├── constants
            │   └── avatarIcons.js # `avatarIcons.js` is used only by `Settings/Avatar`
            ├── Main
            │   └── Entries
            │       └── index.js
            └── Settings
                └── Avatar
                    └── index.js
```

#### ✅ Good

```bash
└── src
    └── components
        └── pages
            ├── Main
            │   └── Entries
            │       └── index.js
            └── Settings
                └── Avatar
                    ├── constants
                    │   └── avatarIcons.js
                    └── index.js
```

`avatarIcons.js` has been moved to the `Settings/Avatar` directory. This indicates to the reader that `avatarIcons.js` is only being used by the files in that folder.

### 4. File and directory naming

1. Start component directories with a capital letter, e.g. `components/pages/Main`
2. Start non-component directories with a small letter, e.g. `src/constants`, `src/utils`
3. Use "camelCase" for non-index file names (anything else than `index.*`) , e.g. `src/constants/userTypes.js`
4. Files having their dedicated directories should be named `index.*`, e.g. `src/components/pages/Main/index.js`
5. Files being in the same directory as `index.*` should be named appropriately to their role in relation to the index file, e.g `src/components/pages/Main/style.sass`

#### ❌ Bad

```bash
└── src
    ├── Constants # Non-component directory starting with a capital letter
    │   └── user_types.js # "snake_case" file naming convention used instead of "camelCase"
    ├── store
    │   └── Store.js # Redundancy of name. `Store.js` has it's dedicated directory
    └── components
        └── pages
            └── Main
                ├── Utils # Non-component directory starting with a capital letter
                │   └── index.js # `utils` is not a directory dedicated to one file, `index.js` shouldn't be used here
                ├── List
                │   └── List.js # Redundancy of `List` name usage
                ├── chart # Component directory starts with a small letter
                │   └── Chart.js # rRdundancy of `Chart` name usage
                ├── Main.js # Redundancy of `Main` name usage
                ├── MainStyles.sass # Redundancy of `Main` name usage
                └── MainTest.js # Redundancy of `Main` name usage
```

#### ✅ Good

```bash
└── src
    ├── constants
    │   └── userTypes.js
    ├── store
    │   └── index.js
    └── components
        └── pages
            └── Main
                ├── utils
                │   └── findUser.js
                ├── List
                │   └── index.js
                ├── Chart
                │   └── index.js
                ├── index.js
                ├── test.js
                └── style.sass
```

## Example of Implementation

Implementation of the hierarchical structure can be found at https://github.com/petejank/react-typescript-material-boilerplate. It was built using [TypeScript](https://github.com/microsoft/TypeScript), [React](https://github.com/facebook/react), [Redux](https://github.com/reduxjs/redux) and [Material-UI](https://github.com/mui-org/material-ui).

## Contribution

If you have ideas on how to improve this document feel free to share them using the issues section or just by creating a PR.

## License

This work is licensed under [CC-BY-4.0](LICENSE)
