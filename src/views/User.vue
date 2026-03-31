<template>
<div class="account-page">
    <h1 class="title has-text-centered">User Profile</h1>
    <div class="box">

        <div class="field is-grouped is-grouped-left">
            <table class="table is-fullwidth">
                <thead>
                    <th></th>
                    <th></th>
                    <th><button><p class="fa-solid fa-pen"></p></button></th>
                </thead>
                <tbody>
                    <tr>
                        <td>Username</td>
                        <td>: <strong>{{ data.username }}</strong></td>
                        <td><button><p class="fa-solid fa-check"></p></button></td>
                    </tr>
                    <tr>
                        <td>Email</td>
                        <td>: <strong>{{ data.email }}</strong></td>
                        <td><button><p class="fa-solid fa-check"></p></button></td>
                    </tr>
                    <tr>
                        <td>Phone Number</td>
                        <td>: <strong>{{ data.phone }}</strong></td>
                        <td><button><p class="fa-solid fa-check"></p></button></td>
                    </tr>
                    
                </tbody>
            </table>
        </div>

        <div class="field is-grouped is-grouped-right">
            <div class="control">
                <button class="button is-danger" @click="logout">Logout</button>
            </div>
        </div>
    </div>
</div>

</template>

<script>
// import axios from 'axios';
import { supabase } from '@/supabaseClient';

export default {
    name: 'User',
    data() {
        return {
            // User data can go here
            data : []
            }
        },
    methods: {
        async logout() {
            // Clear user data and token
            // axios.defaults.headers.common['Authorization'] = '';
            const {error} = await supabase.auth.signOut()
            localStorage.removeItem('token');
            localStorage.removeItem('username');
            localStorage.removeItem('user_id');

            this.$store.commit('clearToken');
            this.$store.state.isAuthenticated = false;

            this.$router.push('/');

        },
        async getUserData() {
            const { data } = await supabase.auth.getUser();

            const id = data.user.identities[0].identity_data;
            const phone = data.user.phone;
            id.phone = phone;
            this.data = id;

        },

    },
    mounted() {
        document.title = "User Profile - Arla Racing Exhaust Indonesia"
        this.getUserData();
    }
}
</script>