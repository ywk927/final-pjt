<template>
    <div id="box">
        <form @submit.prevent="signin">
            <div>
                <label for="username">아이디</label>
                <input type="text" id="username" v-model="username" />
            </div>
            <div>
                <label for="password">비밀번호</label>
                <input type="password" id="password" v-model="password" />
            </div>
            <button type="submit">로그인</button>
        </form>
    </div>
</template>

<script setup>
import { useAuthStore } from '@/stores/auth'
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const authStore = useAuthStore()
const path = sessionStorage.getItem("CURRENT_PATH");
const username = ref('')
const password = ref('')

const signin = () => {
    authStore.login(username.value, password.value)
        .then(() => {
            if (path === null) {
                router.push('/')
            } else {
            sessionStorage.removeItem("CURRENT_PATH");
            router.push({path: path})
            }
        })
        .catch(err => {
            alert('로그인 실패, 아이디와 비밀번호를 확인하세요')
            console.log(err)
        })
}
</script>

<style scoped>
#box {
    max-width: 400px;
    margin: 4rem auto;
    padding: 1.5rem 2.5rem;
    background-color: #ffffff;
    border-radius: 1rem;
    box-shadow: 0 0 15px rgba(0, 0, 0, 0.1);
}

form div {
    margin-bottom: 1rem;
    background-color: #ffffff;
    padding: 1.5rem;
    border-radius: 1rem;
    box-shadow: 0 0 15px rgba(0, 0, 0, 0.1);
}

label {
    display: block;
    margin-bottom: 0.5rem;
    font-weight: bold;
}

input {
    width: 100%;
    padding: 0.5rem;
    border-radius: 0.375rem;
    border: 1px solid #ced4da;
}

button {
    width: 100%;
    padding: 0.5rem;
    background-color: #198754;
    color: white;
    border: none;
    border-radius: 0.375rem;
}

button:hover {
    background-color: #157347;
}
</style>