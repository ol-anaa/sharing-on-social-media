<template>
  <main>
    <h1>
      Compartilhe a imagem em um dos seguintes <strong>aplicativos:</strong>
    </h1>

    <section class="buttons">

      <button class="geral" v-on:click="shareOnNative">
        <img src="./components/icons/share.png" alt="geral" width="20px" height="auto">
        Geral
      </button>

      <button class="lk" v-on:click="shareOnLinkedinMobile">
        <img src="./components/icons/linkedin.png" alt="lk" width="15px" height="auto">
        Linkedin
      </button>
      
      <button class="face" v-on:click="shareOnFacebook">
        <img src="./components/icons/facebook.png" alt="face" width="15px" height="auto">
        Facebook
      </button>

      <button class="insta" v-on:click="shareOnInstagram">
        <img src="./components/icons/instagram.png" alt="insta" width="15px" height="auto">
        Instagram
      </button>

      <button class="tel" v-on:click="shareOnTelegramMobile">
        <img src="./components/icons/telegram.png" alt="tel" width="15px" height="auto">
        Telegram
      </button>
      
      <button class="wpp" v-on:click="shareOnWhatsAppMobile">
        <img src="./components/icons/whatsapp.png" alt="wpp" width="20px" height="auto">
        WhatsApp
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
    async GetCertificated(){
      const response = await fetch(certificado);
      const blob = await response.blob();

      return blob;
    },

    async shareOnNative() {
      await navigator.share({
        title: "Meu Certificado",
        text: "Confira meu certificado!",
        url: this.linkCertificadoAWS,
      });
    },

    async shareOnWhatsAppMobile() {
      if (this.isMobile) 
      {
        this.shareOnNative();
      } 
      else {
        let blob = await this.GetCertificated();
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

      const OrganizationId = 1111;
      let IssueYear = 2024;
      let IssueMonth = 6;
      let ValidationURL = 'https://blog-static.petlove.com.br/wp-content/uploads/2021/08/Gato-filhote-7.jpg';
      let EnrollmentHash = 10101;
      let CertificateName = 'Certificado 01';
      let url = `https://www.linkedin.com/profile/add?startTask=CERTIFICATION_NAME&organizationId=${OrganizationId}&issueYear=${IssueYear}&issueMonth=${IssueMonth}&certUrl=${ValidationURL}&certId=${EnrollmentHash}&name=${CertificateName}`
      
      const appStoreUrl = 'https://apps.apple.com/app/linkedin/id288429040';
      const playStoreUrl = 'https://play.google.com/store/apps/details?id=com.linkedin.android';

      window.open(url, '_blank');

      if (this.isMobile) 
      {
        setTimeout(() => {
            if (!document.hidden) 
              window.open(this.isIOS ? appStoreUrl : playStoreUrl, '_blank');
        }, 500);
      } 
    },

    async shareOnTelegramMobile() {    
      
      const appStoreUrl = 'https://apps.apple.com/app/telegram-messenger/id686449807';
      const playStoreUrl = 'https://play.google.com/store/apps/details?id=org.telegram.messenger';
      const telegramWebUrl = `https://t.me/share/url?url=${encodeURIComponent(this.linkCertificadoAWS)}`;

      window.open(telegramWebUrl, '_blank');

      if (this.isMobile) 
      {
        setTimeout(() => {
            if (!document.hidden) 
              window.open(this.isIOS ? appStoreUrl : playStoreUrl, '_blank');
        }, 500);
      } 
    },
    
    async shareOnInstagram() {
      if (this.isMobile) 
      {
        this.shareOnNative();
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
    },

    async shareOnFacebook() {
      const shareUrl = `https://www.facebook.com/sharer/sharer.php?u=${encodeURIComponent(this.linkCertificadoAWS)}`;

      if (this.isMobile) 
      {
        const appStoreUrl = 'https://apps.apple.com/app/facebook/id284882215';
        const playStoreUrl = 'https://play.google.com/store/apps/details?id=com.facebook.katana'; 
        
        window.open(shareUrl, '_blank');

        setTimeout(() => {
          if (!document.hidden) {

            if (this.isIOS) {
              window.location.href = appStoreUrl;
            }
            else {
              window.location.href = playStoreUrl;
            }
          }
        }, 500);
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

.buttons{
  display: flex;
  flex-direction: row;
  gap:10px;
}

@media only screen and (max-width: 767px)
{
  .buttons{
    display: flex;
    flex-direction: column;
    align-items: start;
    gap:10px;
  }
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

.face{
  background-color: rgb(46, 91, 216);
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
