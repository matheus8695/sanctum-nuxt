<script setup lang="ts">
    const form = ref({
        name: "Joe Doe",
        email: "joe@doe.com",
        password: "password"
    })

    async function handleRegister() {
        await $fetch("http://localhost:8000/sanctum/csrf-cookie", {
            credentials: "include"
        })

        const token = useCookie('XSRF-TOKEN');

        const response = await $fetch("http://localhost:8000/register", {
            credentials: "include",
            method: "POST",
            watch: false,
            body: form.value,
            headers: {
                'X-XSRF-TOKEN': token.value as string
            }
        })
    }
</script>

<template>
  <div>
    Page: register
    <br>

    <form @submit.prevent="handleRegister">
        <input name="name" v-model="form.name">
        <input name="email" v-model="form.email">
        <input name="password" v-model="form.password">

        <button>Register</button>
    </form>
  </div>
</template>

<style scoped></style>
