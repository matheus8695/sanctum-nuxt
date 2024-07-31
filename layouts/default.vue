<script setup lang="ts">
    const user = ref({})

    await $fetch("http://localhost:8000/sanctum/csrf-cookie", {
        credentials: "include"
    })

    const token = useCookie('XSRF-TOKEN');

    async function loadUser() {
        const response =  await $fetch("http://localhost:8000/user", {
            credentials: "include",
            redirect: "manual",
            watch: false,
            ignoreResponseError: true,
            headers: {
                'X-XSRF-TOKEN': token.value as string
            }
        })
        user.value = response as object;
    }

    async function handleLogout() {
        await $fetch("http://localhost:8000/sanctum/csrf-cookie", {
            credentials: "include"
        })

        const token = useCookie('XSRF-TOKEN');

        await $fetch("http://localhost:8000/logout", {
            credentials: "include",
            method: "POST",
            watch: false,
            headers: {
                'X-XSRF-TOKEN': token.value as string
            }
        })

        navigateTo('/')
    }
</script>

<template>
  <div>
    <button @click="loadUser">LoadUser</button>
    <button v-if="user" @click="handleLogout">Logout</button>
    <pre>{{ user }}</pre>
    <ul>
        <li><NuxtLink to="/"">Home</NuxtLink></li>
        <li><NuxtLink to="/login">Login</NuxtLink></li>
        <li><NuxtLink to="/register">Register</NuxtLink></li>
    </ul>
    <slot />
  </div>
</template>

<style scoped></style>
