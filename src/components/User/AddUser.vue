<template>
    <div v-if="loading">

    </div>
    <form class="mt-4" @submit.prevent="submitForm" v-if="!loading">
        <div class="mb-3">
            <label class="form-label">
                Adınız
            </label>
            <input type="text" class="form-control" v-model="user.firstName">
        </div>
        <div class="mb-3">
            <label class="form-label">
                Soyadınız
            </label>
            <input type="text" class="form-control" v-model="user.lastName">
        </div>
        <button @click="submitForm" type="submit" class="btn btn-primary">
            Submit
        </button>
    </form>
</template>

<script setup>
    import { reactive,ref } from 'vue';
    import Axios from 'axios';
    import { useToast } from 'vue-toast-notification';

    const loading = ref(false);

    const user = reactive({
        firstName:'',
        lastName:''
    })

    const submitForm = () => {
        loading.value = true;

        Axios({
            method:"POST",
            url:'http://localhost:3000/users',
            data:user
        })
        .then(()=>{
            $toast.success('Kullanıcı Kaydı Başarılı!');
        })
        .cathch(()=>{
            $toast.error('Kullanıcı Kaydı Başarısız!');
        })
        .finally(()=>{
            loading.value = false;
        })
    }
</script>