<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar color="secondary">
        <ion-title>Lluitador del temps</ion-title>

        <ion-buttons slot="primary">
          <ion-button @click="info">
            <ion-icon :icon="helpCircle"></ion-icon>
          </ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>
    
    <ion-content :fullscreen="true" background="secondary-contrast">
      <ion-header class="ion-no-border ion-padding-top">
        <ion-grid>
          <ion-row>
            <ion-col>
              <div id="punt">
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
        <ion-button id="tapMeButton" @click="tap">
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
import { createAnimation } from '@ionic/vue';

const INITIAL_TIME = 60

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
  data(){
    return{
      helpCircle: helpCircle,
      score: 0,
      timeLeft: INITIAL_TIME,
      notStarted: false,
      counterInterval: null
    }
  },
  methods:{
    bounce(){
      const animation = createAnimation()
      animation.addElement(document.getElementById('tapMeButton'))
        .duration(2000)
        .fromTo('transform', 'scale(2.0)', 'scale(1.0)')
      animation.play();
    },
    blink(){
      const animation = createAnimation()
      animation.addElement(document.getElementById('punt'))
          .duration(500)
          .fromTo('opacity', '0', '1')
      animation.play();
    },
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
    tap() {
      this.bounce()
      this.blink()
      this.score++;
      if(!this.notStarted){
        this.notStarted = true;
        this.counterInterval = setInterval(() => {this.timeLeft--}, 1000)
      }
    },
    async showFinalMsg() {
      const toast = await toastController
          .create({
            message: `Fi del temps! La teva puntuació ha estat de ${this.score}`,
            duration: 2000
          })
      return toast.present();
    }
  },
  watch: {
    // eslint-disable-next-line vue/no-arrow-functions-in-watch
    timeLeft: function(newTimeLeft) {
      if (newTimeLeft <= 0){
        this.notStarted = false;
        this.timeLeft = INITIAL_TIME
        clearInterval(this.counterInterval)
        this.showFinalMsg()
        this.score = 0
      }
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