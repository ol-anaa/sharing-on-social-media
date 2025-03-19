<!--
<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
</script>
-->


<template>
  <main>
    <h1>
      Compartilhe a imagem em um dos <strong>aplicativos:</strong>
    </h1>

    <section class="buttons">

      <button class="geral" v-on:click="shareOnNative">
        <img src="./components/icons/share.png" alt="geral" width="20px" height="auto">
        Geral
      </button>

      <button class="wpp" v-on:click="shareOnWhatsAppMobile">
        <img src="./components/icons/whatsapp.png" alt="wpp" width="20px" height="auto">
        WhatsApp
      </button>

      <button class="lk" v-on:click="shareOnLinkedinMobile">
        <img src="./components/icons/linkedin.png" alt="lk" width="15px" height="auto">
        Linkedin
      </button>

      <button class="tel" v-on:click="shareOnTelegramMobile">
        <img src="./components/icons/telegram.png" alt="tel" width="15px" height="auto">
        Telegram
      </button>

      <button class="insta" v-on:click="shareOnInstagram">
        <img src="./components/icons/instagram.png" alt="insta" width="15px" height="auto">
        Instagram
      </button>

    </section>

  </main>
</template>

<script>
import certificado from '@/assets/certificado.png';

export default {

	name: 'App',
  data(){
    return {
      isMobile: /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent),
      isIOS: /iPad|iPhone|iPod/.test(navigator.userAgent) && !window.MSStream,
      linkCertificadoAWS: "https://blog-static.petlove.com.br/wp-content/uploads/2021/08/Gato-filhote-7.jpg"
    }
  },
	methods:
  {
    //Gerando certificado via blob
    async GetCertificated(){
      const response = await fetch(certificado);
      const blob = await response.blob();

      return blob;
    },

    async shareOnNative() {
      let blob = await this.GetCertificated();
      let file = new File([blob], "certificado.png", { type: "image/png" });
     
      await navigator.share({
        files: [file],
        text: "Confira meu certificado!",
      });
    },

    async shareOnWhatsAppMobile() {
      let blob = await this.GetCertificated();
      let file = new File([blob], "certificado.png", { type: "image/png" });
     
      if (this.isMobile) {
        await navigator.share({
          files: [file],
          text: "Confira meu certificado!",
        });
      } 
      else {
        let url = URL.createObjectURL(blob);
        let link = document.createElement("a");

        link.href = url;
        link.download = "certificado.png";
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);

        window.open("https://web.whatsapp.com/", "_blank");
      }
    },

    async shareOnLinkedinMobile() {

      if(this.isMobile)
      {
        const appStoreUrl = 'https://apps.apple.com/app/id304916183';
        const playStoreUrl = 'https://play.google.com/store/apps/details?id=com.linkedin.android';

        window.location.href = `linkedin://shareArticle?url=${encodeURIComponent(imageUrl)}`;
        
        setTimeout(() => {
          if (!document.hidden) 
            window.location.href = this.isIOS ? appStoreUrl : playStoreUrl;
        }, 500);
      }
      else
      {
        const linkedinShareUrl = `https://www.linkedin.com/sharing/share-offsite/?url=${encodeURIComponent(this.linkCertificadoAWS)}`;
        window.open(linkedinShareUrl, '_blank');
      }

    },

    async shareOnTelegramMobile() {      
      if (this.isMobile) 
      {
        const appStoreUrl = 'https://apps.apple.com/app/telegram-messenger/id686449807';
        const playStoreUrl = 'https://play.google.com/store/apps/details?id=org.telegram.messenger';

        window.location.href = `https://t.me/share/url?url=${encodeURIComponent(this.linkCertificadoAWS)}`;

        setTimeout(() => {
            if (!document.hidden) 
              window.location.href = this.isIOS ? appStoreUrl : playStoreUrl;
        }, 500);
      } 
      else {
        const telegramWebUrl = `https://t.me/share/url?url=${encodeURIComponent(this.linkCertificadoAWS)}`;
        window.open(telegramWebUrl, '_blank');
      }
    },
    
    async shareOnInstagram() {

      window.location.href = `instagram://library?AssetPath=${this.linkCertificadoAW}`;


      /*
      if(this.isMobile)
      {
        const appSotore = "https://apps.apple.com/app/instagram/id389801252";
        const playStore = "https://play.google.com/store/apps/details?id=com.instagram.android";



        if (this.isIOS) {
          window.location.href = `instagram://library?LocalIdentifier=${this.linkCertificadoAW}`;
        } else {
          window.location.href = `intent://#Intent;package=com.instagram.android;action=android.intent.action.SEND;type=image/*;S.android.intent.extra.STREAM=${imageUri};end;`;
        }
        setTimeout(() => {
          window.location.href = this.isIOS ? appSotore : playStore;
        }, 3000);
      }
      else{
        let blob = await this.GetCertificated();
        let url = URL.createObjectURL(blob);

        let link = document.createElement("a");
        link.href = url;
        link.download = "certificado.png";
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);

        window.open("https://www.instagram.com", "_blank");
      }
        */
    }

  }
}
  
</script>


<style scoped>
header {
  line-height: 1.5;
}

h1 {
  font-size: 20px;
}

strong{
  font-weight: bold;
}

main{
  display: flex;
  flex-direction: column;
  gap: 10px;
}

button{
  display: flex;
  flex-direction: row;
  align-items: center;
  gap:5px;

  font-size: 15px;
  font-weight: 500;

  border-radius: 5px;
  border: none;
  padding: 6px 8px;

  cursor: pointer;
}

.buttons{
  display: flex;
  flex-direction: row;
  gap:10px;
}
.geral{
  background-color: gray;
  color: rgb(32, 32, 32);
}

.wpp{
  background-color: green;
  color: rgba(255, 255, 255, 0.808);
}

.lk{
  background-color: rgb(16, 101, 197);
  color: rgba(255, 255, 255, 0.808);
}

.tel{
  background-color: rgb(42, 114, 196);
  color: rgba(255, 255, 255, 0.808);
}

.insta{
  background-color: rgb(216, 46, 188);
  color: rgba(255, 255, 255, 0.808);
}
.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
