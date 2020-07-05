<template>
    <div class="container">
        <div class="row mt-4" v-if="!secrets.length">
            <div class="col-6 offset-3">
                <form action="#" @submit.prevent="handleLogin">
                    <h3>Sign in for secrets</h3>
                    <div class="form-row">
                        <input type="email" name="email" class="form-control" v-model="formData.email" placeholder="Email Address">
                    </div>
                    <div class="form-row">
                        <input type="password" name="password" class="form-control" v-model="formData.password" placeholder="Password">
                    </div>
                    <div class="form-row">
                        <button type="submit" class="btn btn-primary">Sign In</button>
                    </div>
                </form>
            </div>
        </div>

        <div class="row mt-4" v-if="secrets.length">
            <div class="col-6 offset-3">
                <h3>My Secrets</h3>
                <div class="secret" v-for="(secret, index) in secrets" :key="index">
                    <em v-text="secret.created_at"></em>
                    <strong v-text="secret.secret"></strong>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                secrets: [],
                formData: {
                    email: '',
                    password: ''
                }
            }
        },
        methods: {
            async handleLogin() {
                await axios.get('/sanctum/csrf-cookie');
                await axios.post('/login', this.formData);
                await this.getSecrets();
            },
            async getSecrets() {
                try {
                    const response = await axios.get('/api/secrets')
                    console.log(response)
                    this.secrets = response.data;
                } catch (e) {
                    console.log(e)
                }
            }
        }
    }
</script>

<style>
    .form-row {
        margin-bottom: 8px;
    }
</style>
