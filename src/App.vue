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

      <button class="geral" v-on:click="shareOnMobile">
        <img src="./components/icons/share.png" alt="wpp" width="20px" height="auto">
        Geral
      </button>

      <button class="wpp" v-on:click="shareOnWhatsAppMobile">
        <img src="./components/icons/whatsapp.png" alt="wpp" width="20px" height="auto">
        WhatsApp
      </button>

      <button class="lk" v-on:click="shareOnLinkedinMobile">
        <img src="./components/icons/linkedin.png" alt="wpp" width="15px" height="auto">
        Linkedin
      </button>

      <button class="tel" v-on:click="shareOnTelegramMobile">
        <img src="./components/icons/telegram.png" alt="wpp" width="15px" height="auto">
        Telegram
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
      isIOS: /iPad|iPhone|iPod/.test(navigator.userAgent) && !window.MSStream
    }
  },
	methods:
  {
    async GetCertificated(){
      const response = await fetch(certificado);
      const blob = await response.blob();

      return blob;
    },

    async shareOnMobile() {
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
      let blob = await this.GetCertificated();
      let imageUrl = URL.createObjectURL(blob);

      /*
      if(this.isMobile)
      {
        const appStoreUrl = 'https://apps.apple.com/app/id304916183'; // LinkedIn na App Store
        const playStoreUrl = 'https://play.google.com/store/apps/details?id=com.linkedin.android'; // LinkedIn na Play Store
        
        const linkedInAppUrl = `linkedin://shareArticle?url=${encodeURIComponent(imageUrl)}`;
        
        window.location.href = linkedInAppUrl;
  
        setTimeout(() => {
            if (!document.hidden) {
                const isIOS = /iPad|iPhone|iPod/.test(navigator.userAgent) && !window.MSStream;
                const storeUrl = isIOS ? appStoreUrl : playStoreUrl;
                window.location.href = storeUrl;
            }
        }, 500);
      }
      else{
        console.log(imageUrl)
        const linkedInShareUrl = `https://www.linkedin.com/sharing/share-offsite/?url=${encodeURIComponent(imageUrl)}`;
        window.open(linkedInShareUrl, '_blank');
      }
      */
    },

    async shareOnTelegramMobile() {
      const botToken = '7436286567:AAG_J8tpUG8gioQYD2waPeXImGxaaKViLVM'; // Token do bot
      const chatId = '1720763140'; // chat_id do usuário
      
      let blob = await this.GetCertificated();
      let imageUrl = URL.createObjectURL(blob);

      if (this.isMobile) 
      {
        const appStoreUrl = 'https://apps.apple.com/app/telegram-messenger/id686449807';
        const playStoreUrl = 'https://play.google.com/store/apps/details?id=org.telegram.messenger';

        window.location.href = `tg://msg_url?url=${encodeURIComponent(imageUrl)}`;

        setTimeout(() => {
            if (!document.hidden) 
              window.location.href = this.isIOS ? appStoreUrl : playStoreUrl;
        }, 500);
      } 
      else {
        let formData = new FormData();

        formData.append('chat_id', chatId);
        formData.append('document', blob, 'certificado.png');

        try {
          const response = await fetch(`https://api.telegram.org/bot${botToken}/sendDocument`, {
              method: 'POST',
              body: formData,
          });

          let result = await response.json();

          if (!result.ok) 
              console.error('Erro ao enviar o certificado:', result.description);

        } catch (error) {
            console.error('Erro ao enviar o certificado:', error);
        }
      }
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
