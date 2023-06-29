<template>
  <nav>
    <router-link :to="{name: 'home'}">Inicio</router-link> |
    <router-link :to="{name: 'todos.index'}">Todos</router-link>
  </nav>
  <router-view/>
</template>

