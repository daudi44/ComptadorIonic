<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Lluitador del temps</ion-title>

        <ion-buttons slot="primary">
          <ion-button @click="info">
            <ion-icon :icon="helpCircle"></ion-icon>
          </ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>
    
    <ion-content :fullscreen="true">
      <ion-header class="ion-no-border ion-padding-top">
        <ion-grid>
          <ion-row>
            <ion-col>
              <div>
                Your Score: {{score}}
              </div>
            </ion-col>
            <ion-col>
              <div class="ion-text-end">
                Time Left: {{timeLeft}}
              </div>
            </ion-col>
          </ion-row>
        </ion-grid>
      </ion-header>
    
      <div id="container">
        <ion-button @click="tap">
          TAP ME!
        </ion-button>
      </div>
    </ion-content>
  </ion-page>
</template>

<script>
import {
  alertController,
  IonButton,
  IonButtons, IonCol,
  IonContent, IonGrid,
  IonHeader,
  IonIcon,
  IonPage, IonRow,
  IonTitle,
  IonToolbar, toastController
} from '@ionic/vue';
import { defineComponent } from 'vue';
import { helpCircle } from "ionicons/icons";
export default defineComponent({
  name: 'Home',
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonButton,
    IonButtons,
    IonIcon,
    IonGrid,
    IonRow,
    IonCol
  },
  setup(){
    return{
      helpCircle: helpCircle,
      score: 0,
      timeLeft: 60
    }
  },
  methods:{
    async info(){
      const alert = await alertController
          .create({
            cssClass: 'my-custom-class',
            header: 'Lluitador del temps',
            subHeader: 'Creat per Daniel Audí Bielsa',
            message: 'Codi font <a href="https://github.com/daudi44/ComptadorIonic">aquí<a/> ',
            buttons: ['OK'],
          });
      await alert.present();

      const { role } = await alert.onDidDismiss();
      console.log('onDidDismiss resolved with role', role);
    },
    async tap() {
      const toast = await toastController
          .create({
            message: 'Your settings have been saved.',
            duration: 2000
          })
      return toast.present();
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