<template>
    <div v-if="data.loading">
        <app-loading />
    </div>
    <div class="row" v-if="!data.loading">
        <div class="col-md-3 mb-4" v-for="user in data.users" :key="user.id">
            <div class="card" style="width: 18rem;">
                <img :src="'https://placebeard.it/300/300?' + user.id" class="card-img-top" alt="...">
                <div class="card-body">
                    <p class="card-text">
                        {{user.firstName}} {{ user.lastName }}
                    </p>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
    import axios from 'axios';
    import { onMounted,reactive } from 'vue';
    import Loading from '../Utility/Loading.vue';
    import { useToast } from 'vue-toast-notification';

    const $toast = useToast();

    const data = reactive({
        users:[],
        loading:true
    });

    const getUser = async() => {
        try {
            const response = await axios.get('http://localhost:3000/users');

            data.users = response.data;
            data.loading = false;
        }
        catch (error) {
            $toast.error('You Did Not It!');
            data.loading = false;
        }
    }

    onMounted(()=>{
        getUser()
    })
</script>