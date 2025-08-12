<template>
    <h1>註冊</h1>
    <br>
    <input type="email" placeholder="請輸入Email" v-model="signupField.email">
    <input type="text"  placeholder="請輸入Password" v-model="signupField.password">
    <input type="text"  placeholder="請輸入NickName" v-model="signupField.nickname">
    <br>
    {{ signupField }}
    <button type="button" @click="signup">註冊</button>
    {{ signupRes }}

    <h1>登入</h1>
    <br>
    <input type="email" placeholder="請輸入Email" v-model="signinField.email">
    <input type="text"  placeholder="請輸入Password" v-model="signinField.password">
    <br>
    {{ signinField }}
    <button type="button" @click="signin">登入</button>
    token: {{ signinRes }}

    <h1>驗證</h1>
    <br>
    <div v-if="user.uid">
        <p>UID:{{ user.uid }}</p>
        <p>Nickname:{{ user.nickname }}</p>
    </div>
    <div v-else>
        <p>你還沒登入</p>
    </div>

</template>

<script setup>
    import { onMounted, ref } from 'vue'
    import axios from 'axios';
    const api = 'https://todolist-api.hexschool.io/'
    const signupField = ref({
        email: '',
        password: '',
        nickname: ''
    })
    
    const signupRes = ref('')
    const signup = async() => {
        try {
            const res = await axios.post(`${api}users/sign_up`,signupField.value);
            console.log(res);
            signupRes.value = res.data.uid;
            //console.log("註冊api signup")
            //console.log(`${api}users/sign_up`)
        } catch (error) {
            console.log("錯誤!");
            console.log(error);
        }
    }

    const signinField = ref({
        email: '',
        password: ''
    })

    const signinRes = ref('')
    const signin = async() => {
        try {
            const res = await axios.post(`${api}users/sign_in`,signinField.value);
            console.log(res);
            signinRes.value = res.data.token;
            document.cookie = `customTodoToken= ${res.data.token};path=/` 
        } catch (error) {
            console.log("錯誤!");
            console.log(error);
        }
    }

    // 驗證
    const user = ref({
        nickname: '',
        uid: ''
    })

    onMounted(async() => {
        const token = document.cookie.replace(/(?:(?:^|.*;\s*)customTodoToken\s*\=\s*([^;]*).*$)|^.*$/, "$1");
        console.log("token", token);
        const res = await axios.get(`${api}users/checkout`, {
            headers: {
                Authorization: token
            }
        });
        user.value = res.data;
    });
</script>

<style></style>