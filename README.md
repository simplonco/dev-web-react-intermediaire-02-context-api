---
title: "React Intermédiaire 02 - Context API"
description: "Ressource sur l'utilisation du Context API dans React pour partager des données entre composants."
show_toc: true
---

## Objectifs

- Comprendre quand utiliser un contexte
- Créer un contexte
- Lire les informations d'un contexte

## Introduction

Dans cette ressource, nous allons introduire le concept de Context API dans React. Comme il est indiqué dans la [documentation officielle](https://react.dev/learn/passing-data-deeply-with-context) :

> Habituellement, vous transmettrez des informations d'un composant parent à un composant enfant via des props. Mais transmettre des props peut devenir verbeux et peu pratique si vous devez les transmettre via de nombreux composants entre des grand-parents et leurs petits enfants, ou si de nombreux composants de votre application ont besoin des mêmes informations. Le contexte permet au composant parent de rendre certaines informations disponibles à n'importe quel composant de l'arborescence située en dessous, quelle que soit sa profondeur, sans les transmettre explicitement via les props.

## Pourquoi utiliser un contexte ?

Un concept courant dans React est connu sous le nom de **prop-drilling** : transmettre les props à travers plusieurs niveaux dans la hiérarchie des composants. Certains composants peuvent même ne pas utiliser le prop reçu : ils la transmettent simplement à un enfant dans l'arborescence des composants.

En utilisant un contexte, tu peux stocker un state dans un espace "global". Ce state va être accessible depuis n'importe quel composant connaissant ton contexte.

## Créer un contexte

Pour créer un contexte, nous utilisons `createContext` :

```jsx
import { createContext } from "react";

const ThemeContext = createContext("light");
```

## Fournir un contexte

Pour fournir un contexte, nous utilisons le composant `Provider` :

```jsx
function App() {
  return (
    <ThemeContext.Provider value="dark">
      <MyComponent />
    </ThemeContext.Provider>
  );
}
```

## Consommer un contexte

Pour consommer un contexte, nous utilisons `useContext` :

```jsx
import { useContext } from "react";
import ThemeContext from "./ThemeContext";

function MyComponent() {
  const theme = useContext(ThemeContext);
  return <div>Thème actuel : {theme}</div>;
}
```

## Exemple complet

Voici un exemple complet avec un sélecteur de thème :

```jsx
import { createContext, useContext, useState } from "react";

const ThemeContext = createContext();

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");

  const toggleTheme = () => {
    setTheme(theme === "light" ? "dark" : "light");
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

function App() {
  return (
    <ThemeProvider>
      <Header />
      <Main />
    </ThemeProvider>
  );
}

function Header() {
  const { theme, toggleTheme } = useContext(ThemeContext);
  return (
    <header>
      <p>Thème actuel : {theme}</p>
      <button onClick={toggleTheme}>Changer de thème</button>
    </header>
  );
}

function Main() {
  const { theme } = useContext(ThemeContext);
  return (
    <main style={{ background: theme === "light" ? "#fff" : "#333" }}>
      <p>Contenu principal</p>
    </main>
  );
}
```

## Bonnes pratiques

1. **Ne pas abuser des contextes** : Utilise-les uniquement quand c'est vraiment nécessaire.
2. **Séparer les contextes** : Un contexte par concept (thème, authentification, etc.).
3. **Fournir des valeurs stables** : Éviter de créer de nouveaux objets à chaque rendu.
