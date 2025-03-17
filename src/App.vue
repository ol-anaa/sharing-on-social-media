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
    async shareOnWhatsAppMobile() {
      const response = await fetch(certificado);

      const blob = await response.blob();
      const file = new File([blob], "certificado.png", { type: "image/png" });
      
      if (navigator.canShare && navigator.canShare({ files: [file] })) {
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
  cursor: pointer;
}
.wpp{
  background-color: green;
  display: flex;
  flex-direction: row;
  align-items: center;
  gap:5px;

  font-size: 15px;
  color: rgba(255, 255, 255, 0.808);
  font-weight: 500;

  border-radius: 10px;
  border: none;
  padding: 8px;
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
