<template>
  <footer>
    <div class="footer-section">
      <div class="footer-logo">
        <img src="https://res.cloudinary.com/thibernar/image/upload/v1661379819/LogoTaskApp_gcrtyj.svg" alt="LogoTaskApp">
      </div>
      <div class="footer-separador"></div>
      <div class="footer-text">
        <p>© {{ year }} TaskApp. All rights reserved. Built by Thiago Bernardino.</p>
      </div>
    </div>
  </footer>
</template>

<script setup>
import moment from 'moment'

const year = moment().format('YYYY')

</script>

<style scoped>

.footer-section{
  text-align: center;
}

.footer-logo img{
  width: 200px;
}

.footer-separador{
  border-top: 1px solid rgb(48, 49, 52, 0.1);
  height: 2px;

}

.footer-text{
  color: #303134;
  display: flex;
  flex-direction: row;
  justify-content: center;
}

.footer-text p{
  margin-left: 10px;
}

.footer-text .footer-logo{
  margin-right: 10px;
}

</style>
