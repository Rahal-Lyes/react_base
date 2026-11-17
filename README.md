# react_base

# Les bases de React

Sommaire :

1. Destructuring (objet et tableau)
2. Rest operator
3. Spread operator
4. Template literals
5. Array `map`
6. Array `filter`
7. Array `reduce`
8. Array `sort`
9. Travailler avec des tableaux immuables
10. Components et Props
11. State et Events
12. State Management

---

## 1. Destructuring (déstructuration)

Permet d'extraire des valeurs d'un objet ou d'un tableau dans des variables.

### Objet

```js
const user = { name: "Ali", age: 25, city: "Oran" };

// Sans destructuring
const name1 = user.name;

// Avec destructuring
const { name, age } = user;
console.log(name, age); // Ali 25

// Renommer une variable
const { city: ville } = user;
console.log(ville); // Oran

// Valeur par défaut
const { country = "Algérie" } = user;
console.log(country); // Algérie
```

### Tableau

```js
const colors = ["rouge", "vert", "bleu"];

const [first, second] = colors;
console.log(first, second); // rouge vert

// Ignorer un élément
const [, , third] = colors;
console.log(third); // bleu

// Échanger deux valeurs
let a = 1, b = 2;
[a, b] = [b, a];
```

### Utilisation en React

```jsx
// Destructuring des props
function Welcome({ name, age }) {
  return <p>{name} a {age} ans</p>;
}

// Destructuring de useState
const [count, setCount] = useState(0);
```

---

## 2. Rest operator (`...`)

Regroupe les éléments restants dans un objet ou un tableau.

```js
// Dans un tableau
const [first, ...others] = [1, 2, 3, 4];
console.log(first);  // 1
console.log(others); // [2, 3, 4]

// Dans un objet
const { name, ...rest } = { name: "Ali", age: 25, city: "Oran" };
console.log(rest); // { age: 25, city: "Oran" }

// Dans les paramètres d'une fonction
function sum(...numbers) {
  return numbers.reduce((acc, n) => acc + n, 0);
}
console.log(sum(1, 2, 3, 4)); // 10
```

En React : `function Button({ label, ...props }) { return <button {...props}>{label}</button>; }`

---

## 3. Spread operator (`...`)

Étale les éléments d'un tableau ou d'un objet.

```js
// Copier / fusionner des tableaux
const arr1 = [1, 2];
const arr2 = [3, 4];
const merged = [...arr1, ...arr2]; // [1, 2, 3, 4]

// Copier / fusionner des objets
const user = { name: "Ali", age: 25 };
const updated = { ...user, age: 26, city: "Oran" };
// { name: "Ali", age: 26, city: "Oran" }

// Passer des arguments
Math.max(...[3, 7, 2]); // 7
```

> Rest et spread utilisent la même syntaxe `...` : **rest** *regroupe*, **spread** *étale*.

---

## 4. Template literals

Chaînes de caractères avec des backticks `` ` `` et `${}` pour insérer des expressions.

```js
const name = "Ali";
const age = 25;

// Ancienne méthode
const msg1 = "Je m'appelle " + name + " et j'ai " + age + " ans";

// Template literal
const msg2 = `Je m'appelle ${name} et j'ai ${age} ans`;

// Expressions et multi-lignes
const msg3 = `
  Dans 5 ans, j'aurai ${age + 5} ans.
  ${age >= 18 ? "Majeur" : "Mineur"}
`;
```

En React :

```jsx
<img src={`/images/${user.id}.png`} alt={`Photo de ${user.name}`} />
<div className={`card ${isActive ? "active" : ""}`}>...</div>
```

---

## 5. Array `map`

Transforme chaque élément et retourne un **nouveau tableau** de même taille.

```js
const numbers = [1, 2, 3];
const doubled = numbers.map((n) => n * 2);
console.log(doubled); // [2, 4, 6]

const users = [{ id: 1, name: "Ali" }, { id: 2, name: "Sara" }];
const names = users.map((u) => u.name); // ["Ali", "Sara"]
```

En React (afficher une liste, avec une `key` unique) :

```jsx
function UserList({ users }) {
  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

---

## 6. Array `filter`

Retourne un **nouveau tableau** avec uniquement les éléments qui respectent une condition.

```js
const numbers = [1, 2, 3, 4, 5, 6];
const evens = numbers.filter((n) => n % 2 === 0);
console.log(evens); // [2, 4, 6]

const todos = [
  { id: 1, text: "Apprendre React", done: true },
  { id: 2, text: "Faire un projet", done: false },
];
const pending = todos.filter((t) => !t.done);
```

---

## 7. Array `reduce`

Réduit un tableau à **une seule valeur** (nombre, objet, tableau...).

```js
// array.reduce((accumulateur, élément) => ..., valeurInitiale)
const numbers = [1, 2, 3, 4];
const total = numbers.reduce((acc, n) => acc + n, 0);
console.log(total); // 10

// Panier : total des prix
const cart = [
  { name: "Livre", price: 20 },
  { name: "Stylo", price: 5 },
];
const totalPrice = cart.reduce((acc, item) => acc + item.price, 0); // 25

// Compter les occurrences
const fruits = ["pomme", "poire", "pomme"];
const count = fruits.reduce((acc, f) => {
  acc[f] = (acc[f] || 0) + 1;
  return acc;
}, {});
// { pomme: 2, poire: 1 }
```

---

## 8. Array `sort`

Trie un tableau. **Attention : `sort` modifie le tableau d'origine** (mutation).

```js
// Par défaut : tri alphabétique (convertit en texte !)
[10, 9, 1].sort(); // [1, 10, 9]  ❌

// Tri numérique
[10, 9, 1].sort((a, b) => a - b); // [1, 9, 10] croissant
[10, 9, 1].sort((a, b) => b - a); // [10, 9, 1] décroissant

// Tri de textes
["b", "a", "c"].sort((a, b) => a.localeCompare(b)); // ["a", "b", "c"]

// Tri d'objets
const users = [{ name: "Sara", age: 30 }, { name: "Ali", age: 25 }];
users.sort((a, b) => a.age - b.age);
```

Pour ne pas muter l'original, copier d'abord :

```js
const sorted = [...users].sort((a, b) => a.age - b.age);
```

---

## 9. Travailler avec des tableaux immuables

En React, on **ne modifie jamais** directement le state. On crée un **nouveau tableau**.

| Action    | ❌ À éviter (mutation)   | ✅ À utiliser (immuable)                        |
| --------- | ----------------------- | ---------------------------------------------- |
| Ajouter   | `push`, `unshift`       | `[...arr, item]`                               |
| Supprimer | `splice`, `pop`, `shift`| `arr.filter((x) => x.id !== id)`               |
| Modifier  | `arr[0] = ...`          | `arr.map((x) => x.id === id ? {...x, ...} : x)`|
| Trier     | `arr.sort()`            | `[...arr].sort()`                              |

```js
const todos = [
  { id: 1, text: "A", done: false },
  { id: 2, text: "B", done: false },
];

// Ajouter
const added = [...todos, { id: 3, text: "C", done: false }];

// Supprimer
const removed = todos.filter((t) => t.id !== 2);

// Modifier
const toggled = todos.map((t) =>
  t.id === 1 ? { ...t, done: !t.done } : t
);
```

---

## 10. Components et Props

### Component

Un composant est une **fonction** qui retourne du JSX. Son nom commence par une **majuscule**.

```jsx
function Greeting() {
  return <h1>Bonjour !</h1>;
}

// Utilisation
function App() {
  return (
    <div>
      <Greeting />
      <Greeting />
    </div>
  );
}
```

### Props

Les props sont les **données passées** d'un composant parent à un enfant. Elles sont **en lecture seule**.

```jsx
function UserCard({ name, age, isAdmin = false }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>{age} ans</p>
      {isAdmin && <span>Admin</span>}
    </div>
  );
}

function App() {
  return <UserCard name="Ali" age={25} isAdmin />;
}
```

### `children`

```jsx
function Card({ title, children }) {
  return (
    <div className="card">
      <h3>{title}</h3>
      {children}
    </div>
  );
}

<Card title="Profil">
  <p>Contenu à l'intérieur</p>
</Card>
```

---

## 11. State et Events

### State avec `useState`

Le state est une donnée **qui change** dans le temps. Quand il change, le composant se **re-rend**.

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Compteur : {count}</p>
      <button onClick={() => setCount(count + 1)}>+1</button>
      <button onClick={() => setCount((prev) => prev - 1)}>-1</button>
    </div>
  );
}
```

### Events

Les événements s'écrivent en **camelCase** et reçoivent une **fonction**.

```jsx
function Form() {
  const [text, setText] = useState("");

  function handleChange(e) {
    setText(e.target.value);
  }

  function handleSubmit(e) {
    e.preventDefault(); // empêche le rechargement de la page
    console.log("Envoyé :", text);
    setText("");
  }

  return (
    <form onSubmit={handleSubmit}>
      <input value={text} onChange={handleChange} />
      <button type="submit">Envoyer</button>
    </form>
  );
}
```

### State avec tableau et objet (immuabilité)

```jsx
function TodoApp() {
  const [todos, setTodos] = useState([]);
  const [text, setText] = useState("");

  const addTodo = () => {
    setTodos([...todos, { id: Date.now(), text, done: false }]);
    setText("");
  };

  const deleteTodo = (id) => setTodos(todos.filter((t) => t.id !== id));

  const toggleTodo = (id) =>
    setTodos(todos.map((t) => (t.id === id ? { ...t, done: !t.done } : t)));

  return (
    <div>
      <input value={text} onChange={(e) => setText(e.target.value)} />
      <button onClick={addTodo}>Ajouter</button>
      <ul>
        {todos.map((t) => (
          <li key={t.id}>
            <span
              onClick={() => toggleTodo(t.id)}
              style={{ textDecoration: t.done ? "line-through" : "none" }}
            >
              {t.text}
            </span>
            <button onClick={() => deleteTodo(t.id)}>X</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

---

## 12. State Management

Gérer le state quand plusieurs composants doivent le partager.

### a) Lifting state up (remonter le state)

Le state est placé dans le **parent commun** et passé aux enfants via les props.

```jsx
function Parent() {
  const [count, setCount] = useState(0);

  return (
    <>
      <Display count={count} />
      <Controls onIncrement={() => setCount(count + 1)} />
    </>
  );
}

function Display({ count }) {
  return <p>Valeur : {count}</p>;
}

function Controls({ onIncrement }) {
  return <button onClick={onIncrement}>+1</button>;
}
```

### b) `useReducer` (state complexe)

```jsx
import { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    case "reset":
      return { count: 0 };
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div>
      <p>{state.count}</p>
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
      <button onClick={() => dispatch({ type: "reset" })}>Reset</button>
    </div>
  );
}
```

### c) Context API (éviter le « prop drilling »)

```jsx
import { createContext, useContext, useState } from "react";

const ThemeContext = createContext();

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");
  const toggle = () => setTheme((t) => (t === "light" ? "dark" : "light"));

  return (
    <ThemeContext.Provider value={{ theme, toggle }}>
      {children}
    </ThemeContext.Provider>
  );
}

function ThemeButton() {
  const { theme, toggle } = useContext(ThemeContext);
  return <button onClick={toggle}>Thème : {theme}</button>;
}

function App() {
  return (
    <ThemeProvider>
      <ThemeButton />
    </ThemeProvider>
  );
}
```

### Quand utiliser quoi ?

| Besoin                                      | Solution            |
| ------------------------------------------- | ------------------- |
| State local à un composant                  | `useState`          |
| State partagé entre quelques composants     | Lifting state up    |
| State avec logique complexe                 | `useReducer`        |
| State global (thème, utilisateur connecté)  | Context API         |
| Grande application                          | Redux Toolkit, Zustand |

---

## Résumé

- **Destructuring** : extraire des valeurs (`const { a } = obj`, `const [x] = arr`).
- **Rest / Spread** : `...` regroupe / étale.
- **Template literal** : `` `Bonjour ${name}` ``.
- **map** transforme, **filter** sélectionne, **reduce** calcule une valeur, **sort** trie (mutation !).
- **Immutabilité** : ne jamais modifier le state directement, toujours créer une copie.
- **Components + Props** : des fonctions JSX qui reçoivent des données.
- **State + Events** : `useState` et `onClick`, `onChange`, `onSubmit`.
- **State management** : lifting state up, `useReducer`, Context API.
