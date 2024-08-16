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

    /* reactive Kullanma Amaci 
        Obje Icindeki HTML Elementi Icinde 
        Data Yazdirma Islemi Icin 
        Degisken Adindan Sonra Direkt En Sondaki Attribute Kullanilarak 
        Yazdirma Islemi Yapilabilir */
    /* reactive Kullanildiginda 
        Obje Icindeki Deger Sonradan Degistirilemez */
    /* ref Kullanma Amaci 
        Obje Icindeki HTML Elementi Icinde 
        Data Yazdirma Isleminin Anlik Olarak Yapilmasini Saglar */
    import { reactive,ref } from 'vue';
    import Axios from 'axios';

    /* User.vue Dosyasinda Hata Olursa 
        Mesajla Bildirilmesi Icin Gereken Paketi Kullaniyoruz */
    import { useToast } from 'vue-toast-notification';

    /* Eklenen Data lari Kaydetme Asamasinda Gorunecek Animasyonu 
        Ilk Asamada Kapali Tutuyoruz */
    const loading = ref(false);

    const user = reactive({
        firstName:'',
        lastName:''
    })

    const submitForm = () => {

        /* Ekleme Butonuna Tiklandiginda 
        Animasyonun Gorunmesini Sagliyoruz */
        loading.value = true;

        Axios({
            method:"POST",
            url:'http://localhost:3000/users',
            data:user
        })
        .then(()=>{
            /* Islem Basarili Olursa Verilecek Mesaj */
            $toast.success('Kullanıcı Kaydı Başarılı!');
        })
        .cathch(()=>{
            /* Islem Basarisiz Olursa Verilecek Mesaj */
            $toast.error('Kullanıcı Kaydı Başarısız!');
        })
        .finally(()=>{
            /* Ekleme Islemi Bittiginde 
                Animasyonun Gorunmesini Durduruyoruz */
            loading.value = false;
        })
    }
</script>