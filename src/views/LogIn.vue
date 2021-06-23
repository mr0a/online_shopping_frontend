<template>
    <div class="page-sign-up">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Log in</h1>

                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label for="username">Username</label>
                        <div class="control">
                            <input type="text" id="username" class="input" v-model="username">
                        </div>
                    </div>

                    <div class="field">
                        <label for="password">Password</label>
                        <div class="control">
                            <input type="password" id="password" class="input" v-model="password">
                        </div>
                    </div>

                    <div class="notification is-danger" v-if="errors.length">
                        <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                    </div>

                    <div class="field">
                        <div class="control">
                            <button class="button is-primary">Log in</button>
                        </div>
                    </div>

                    <hr>
                    Or <router-link to="/sign-up/">click here </router-link>to Sign up!
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'

export default {
    name: 'LogIn',
    data() {
        return {
            username: '',
            password: '',
            errors: []
        }
    },
    mounted() {
        document.title = 'Log In | Farm Fresh'
    },
    methods: {
        async submitForm() {
            axios.defaults.headers.common['Authorization'] = ''
            localStorage.removeItem('token')

            this.errors = []

            if (this.username === '') {
                this.errors.push('The username is missing')
            }

            if (this.password === '') {
                this.errors.push('The password is too short')
            }

            if (!this.errors.length) {
                const formData = {
                    username: this.username,
                    password: this.password
                }

                axios
                    .post('/api/v1/token/login', formData)
                    .then( response => {
                        console.log(response);
                        const token = response.data.auth_token

                        this.$store.commit('setToken', token)

                        axios.defaults.headers.common['Authorization'] = 'Token ' + token
                        localStorage.setItem('token', token)

                        const toPath = this.$route.query.to || '/cart'
                        this.$router.push(toPath)

                        toast({
                            message: 'Logged In successfully!',
                            type: 'is-success',
                            dismissible: true,
                            pauseOnHover: true,
                            duration: 2000
                        })
                    })
                    .catch(error => {
                        console.log(error);
                        if (error.response) {
                            for(const property in error.response.data) {
                                this.errors.push(`${property}: ${error.response.data[property]}`)
                            }
                            console.log(JSON.stringify(error.response.data))
                        } else if (error.message) {
                            this.errors.push('Something went wrong. Please try again.')

                            console.log(JSON.stringify(error))
                        }
                    })
            }
        }
    }
}
</script>