<script setup lang="ts"></script>

<template>
  <div>
    <ul>
        <li><NuxtLink to="/"">Home</NuxtLink></li>
        <li><NuxtLink to="/login">Login</NuxtLink></li>
        <li><NuxtLink to="/register">Register</NuxtLink></li>
    </ul>
    <slot />
  </div>
</template>

<style scoped></style>
