<template>
  <ion-page>
    <ion-header id="cabecalho">
      <ion-toolbar>
        <ion-title class="ion-text-center">Calculadora IMC</ion-title>
        <!-- Number type input -->

      </ion-toolbar>
    </ion-header>
    
    <ion-content id="conteudo">
      <ion-fab vertical="top" horizontal="end" id="botaoCalcula">
        <ion-fab-button v-on:click="onCalculate()">
          <ion-icon :icon="calculator"></ion-icon>
        </ion-fab-button>
      </ion-fab>
        <ion-item>
          <ion-label position="floating">Peso (kg)</ion-label>
          <ion-input type="number" id="weight" value="80"></ion-input>
        </ion-item>
        <ion-item>
          <ion-label position="floating">Altura (m)</ion-label>
          <ion-input type="number" id="height" value="1.65" step="0.01"></ion-input>
        </ion-item>
        <ion-toast
          :is-open="isOpenRef"
          :message="message"
          :showCloseButton="true"
        >
        </ion-toast>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonHeader, 
    IonPage, 
    IonTitle, 
    IonToolbar, 
    IonItem, 
    IonInput, 
    IonLabel, 
    IonFab, 
    IonFabButton,  
    IonIcon,
    IonToast,
    toastController,
    IonContent
     } from '@ionic/vue';
import { 
  calculator,
  close
} from 'ionicons/icons';
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Home',
  components: {
    IonFab, 
    IonFabButton, 
    IonIcon,
    IonLabel,
    IonInput,
    IonItem,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonToast,
    IonContent
    
  },
  data(){
    return{
      toastIsOpen: false,
      title: "",
      message : "",

    }
  },
  setup() {
    return {
      calculator
    }
  },
  methods:{
    decideMessage: function(valor){
      if(valor < 18.5){
        return "abaixo do normal."
      }
      else if(valor < 25){
        return "normal."
      }
      else if(valor < 30){
        return "acima do peso/pré obesidade."
      }
      else if(valor < 35){
        return "obesidade grau um."
      }
      else if(valor < 40){
        return "obesidade grau dois."
      }
      else{
        return "obesidade grau trés ou morbida."
      }
    },

    onCalculate: function(){
      if(!this.toastIsOpen){
        const alturaHtml = document.getElementById('height') as HTMLInputElement
        const pesoHtml = document.getElementById('weight') as HTMLInputElement

        const altura = +(alturaHtml.value);
        const peso = +(pesoHtml.value);
        const imc = peso/(altura*altura)
        console.log("O seu imc é de:", (imc.toFixed(1)).toString())
        this.title ="O seu imc é de: " + (imc.toFixed(1)).toString()
        this.message = "Isso significa que o seu peso é considerado " + this.decideMessage(imc)
        this.openToastOptions()
        this.toastIsOpen = true
      }
      else{
        alert("Feche a mensagem na parte de baixo da página")
      }
    },
    async openToastOptions() {
      const toast = await toastController
        .create({
          header: this.title,
          message: this.message,
          position: 'bottom',
          buttons: [ {
              icon: close,
              role: 'cancel',
              handler: () => {
                this.toastIsOpen = false

                console.log('Cancel clicked');
              }
            }
          ]
        })
      await toast.present();

      const { role } = await toast.onDidDismiss();
      console.log('onDidDismiss resolved with role', role);
    },
      
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

#cabecalho{
  position: relative;
  z-index: 100;
}

#conteudo{
  position: relative;
  z-index: 200;
}

#icone{
  position: relative;
  z-index: 400;
}

#botaoCalcula{
  position: fixed;
  z-index: 300;
}
</style>