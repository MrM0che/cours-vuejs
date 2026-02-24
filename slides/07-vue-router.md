---
marp: true
paginate: true
footer: Adrien Bouyssou (macdrien.github.io)
---

# Vue-Router

---

## Utilité

Permet de naviguer au sein de l'application VueJS et de gérer l'historique.

### Paquet NPM

`"vue-router": "^X.Y.Z"`

---

## Setup du router - Router

```javascript
import { createMemoryHistory, createRouter } from "vue-router";

import HomeView from "./HomeView.vue";
import AboutView from "./AboutView.vue";

const routes = [
  { path: "/", component: HomeView },
  { path: "/about", component: AboutView },
];

export const router = createRouter({
  history: createMemoryHistory(),
  routes,
});
```

---

## Setup du router - App

```javascript
import { router } from "./router";

createApp(App).use(router).mount("#app");
```

---

## Définition des routes

Asspcoer la route dans l'URL et le composant à afficher.

```js
const routes = [
  { path: "/", component: HomeView },
  { path: "/about", component: AboutView },
];
```

---

## Routes dynamiques

```js
const routes = [
  { path: "/users", component: UsersView },
  { path: "/user/:id", component: UserView },
];
```

---

## Routes dynamiques - Regex

```js
const routes = [
  { path: "/:id(\\d+)", component: CategoryView },
  { path: "/:slug", component: ArticleView },
];
```

---

## Arguments d'une route

- `name`: Nomme la route. Limite le risque d'erreur et facilite les refactors
- `props`: Permet de passer des props au composant ciblé
- `children`: Permet de définir des routes enfants

---

## Utilisation des arguments

```js
const routes = [
  {
    path: "/users"
    component: UsersView,
    name: "users",
    props: route => ({ query: route.query.q }),
    children: [
      {
        path: ":id",
        component: UserView,
        name: "user-details",
        children: [
          { path: "edit", name: "user-edit", component: UserEditView }
        ],
      }
    ]
  }
];
```

---

## Injection du router dans l'application

```js
import { createApp } from "vue";
import { router } from "./router";
import App from "./App.vue";

const app = createApp(App);
app.use(router);
app.mount("#app");
```

---

## Déclaration du router

```html
<template>
  <!-- On est dans App.vue, au milieu du template -->
  <router-view />
</template>
```

---

## Navigation programmatique - router-link

```html
<template>
  <nav>
    <router-link to="/">Home</router-link>
    <router-link to="/about">About</router-link>
  </nav>
</template>
```

---

## Navigation programmatique - router.push

```js
import { useRouter } from "vue-router";
export default {
  setup() {
    const router = useRouter();

    function goToAbout() {
      // router.push("/about");
      router.push({ name: "about" });
    }

    return { goToAbout };
  },
};
```

## Navigation programmatique - router.push avec props

```js
import { useRouter } from "vue-router";
export default {
  setup() {
    const router = useRouter();

    function goToUser(id) {
      router.push({ name: "user-details", params: { id } });
      // /users/123
    }

    function searchUser(query) {
      router.push({ name: "users", query: { q: query } });
      // /users?q=john
    }

    return { goToUser, searchUser };
  },
};
```

---

## Navigation programmatique - replace

```js
import { useRouter } from "vue-router";
export default {
  setup() {
    const router = useRouter();

    function switchToAbout() {
      router.replace("/about");
    }
    return { switchToAbout };
  },
};
```

---

## Navigation programmatique - retour en arrière

```js
import { useRouter } from "vue-router";
export default {
  setup() {
    const router = useRouter();

    function goBack() {
      router.back();
      // router.go(-1);
    }
    return { goBack };
  },
};
```
