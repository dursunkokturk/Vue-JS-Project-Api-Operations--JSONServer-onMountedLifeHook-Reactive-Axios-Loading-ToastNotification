<template>

    <!-- Data Gelene Kadar Ekranda Animasyon Gosteriyoruz -->
    <div v-if="data.loading">

        <!-- Main.js Dosyasindaki Animasyonu Kullanmak Icin
            Component Olarak Cagiriyoruz -->
        <app-loading />
    </div>

    <!-- Data Gelme Islemi Bittikten Sonra 
        Data lari Listeliyoruz -->
    <div class="row" v-if="!data.loading">
        <div class="col-md-3 mb-4" v-for="user in data.users" :key="user.id">
            <div class="card" style="width: 18rem;">

                <!-- Rastgele Fotograflarin Alinacagi Sitenin Linkinden Sonra 
                    Fotograflarin Boyut Bilgilerini Giriyoruz
                    + Isaretinden Sonra db.json Dosyasinda Bulunan ve 
                    user Degiskeni Uzerinden Ulastigimiz Data lari 
                    id Bilgini Kullanarak Ekrana Yazdiriyoruz -->
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

    /* User.vue Dosyasinda Hata Olursa 
        Mesajla Bildirilmesi Icin Gereken Paketi Kullaniyoruz */
    import { useToast } from 'vue-toast-notification';

    const $toast = useToast();

    const data = reactive({
        users:[],
        loading:true
    });

    const getUser = async() => {
        try {

            /* Data lari Almak Icin Request Atiyoruz */
            const response = await axios.get('http://localhost:3000/users');

            /* Gelen Data lari data Degiskeni Uzerinden Yazdiriyoruz */
            data.users = response.data;

            /* Data Gelmeye Baslayinca Animasyonu Durduruyoruz */
            data.loading = false;
        }
        catch (error) {
            /* Islem Basarili Olursa Verilecek Mesaj */
            $toast.error('You Did Not It!');

            /* Data Gelmeye Baslayana Kadar Hata Olusmadi Ise 
                Hata Almayi Durduruyoruz */
            data.loading = false;
        }
    }

    onMounted(()=>{
        getUser()
    })
</script>