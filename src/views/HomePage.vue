<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Blank</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-list v-for="item in payload" :key="item">
        <ion-item>
          <ion-avatar slot="start">
            <img :src="getImgUrl(item.logoImage)" :alt="item.name" />
          </ion-avatar>
          <ion-label>
            <h2>{{ item.name }}</h2>
          </ion-label>
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar } from '@ionic/vue';
import { defineComponent } from 'vue';
import axios from 'axios'

export default defineComponent({
  name: 'HomePage',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar
  },
  data() {
    return {
      payload: [],
    }
  },
  mounted() {
    fetch("https://prototype-mcontent-registered3rd-hooks.azurewebsites.net/prototype/GetCurrentlyOpenedRestaurants").then(response => {
      return response.json();
    }).then(data => {
      this.payload = data.map((res: any) => {
        const picname: string = res.logoImage.split("/")[2]  // แยกเอาแค่ชื่อไฟล์ออกมา
        const pic : string = picname.split(".")[0] // แยกเอาสกุลออก
        return {
          id: res.id,
          name: res.name,
          logoImage: pic
        }
      })
    });
  },
  methods: {
    getImgUrl(pet:string) { // เรียกใช้รูปใน :src
      var images = require.context('../assets/imgs', false, /\.png$/)
      return images('./' + pet + ".png")
    }
  }
});
</script>

<style scoped>
#container {
  text-align: center;

  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;

  color: #8c8c8c;

  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>
