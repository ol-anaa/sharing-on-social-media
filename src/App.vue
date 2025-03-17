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

    </section>

  </main>
</template>

<script>
import certificado from '@/assets/certificado.png';

export default {

	name: 'App',
	methods:
  {
    async shareOnMobile() {
      const response = await fetch(certificado);

      const blob = await response.blob();
      const file = new File([blob], "certificado.png", { type: "image/png" });
     
      await navigator.share({
        files: [file],
        text: "Confira meu certificado!",
      });
    },

    async shareOnWhatsAppMobile() {
      const response = await fetch(certificado);

      const blob = await response.blob();
      const file = new File([blob], "certificado.png", { type: "image/png" });
     
      const isMobile = /iPhone|iPad|iPod|Android/i.test(navigator.userAgent);

      if (isMobile) {
        await navigator.share({
          files: [file],
          text: "Confira meu certificado!",
        });
      } 
      else {
        const url = URL.createObjectURL(blob);
        const link = document.createElement("a");

        link.href = url;
        link.download = "certificado.png";
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);

        window.open("https://web.whatsapp.com/", "_blank");
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
