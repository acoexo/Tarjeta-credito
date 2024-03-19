<template>
    <!-- Estructura principal de la página -->
    <div class="card">
        <!-- Componente para mostrar la cara delantera de la tarjeta de crédito -->
        <TarjetaCreditoFront :cardNumber="formData.cardNumber" :holderName="formData.holderName"
            :expDateMonth="formData.expDateMonth" :expDateYear="formData.expDateYear" />
        
        <!-- Componente para mostrar la cara trasera de la tarjeta de crédito -->
        <TarjetaCreditoBack :cardCVC="formData.cardCVC" />
    </div>

    <!-- Componente del formulario para ingresar los detalles de la tarjeta de crédito -->
    <TarjetaCreditoForm @form-submitted="handleFormSubmitted" />

    <!-- Atribución del desafío -->
    <div class="attribution">
        Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank">Frontend Mentor</a>.
        Coded by <a href="#">acoexo</a>.
    </div>
</template>

<script setup>
import { ref } from 'vue';
import TarjetaCreditoForm from './components/TarjetaCreditoForm.vue'; 
import TarjetaCreditoFront from './components/TarjetaCreditoFront.vue'; 
import TarjetaCreditoBack from './components/TarjetaCreditoBack.vue'; 

// Variable reactiva para almacenar los datos del formulario
const formData = ref({ 
  cardNumber: '', // Número de tarjeta
  holderName: '', // Nombre del titular
  expDateMonth: '', // Mes de vencimiento
  expDateYear: '', // Año de vencimiento
  cardCVC: '' // Código CVC
});

// Función que se ejecuta cuando se envía el formulario
const handleFormSubmitted = (data) => {
  formData.value = data; // Actualizar los datos del formulario con los datos recibidos
};
</script>


<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300..700&display=swap');
$font-stack: "Space Grotesk", sans-serif;

$linear-gradient: linear-gradient(hsl(249, 99%, 64%) to hsl(278, 94%, 30%));
$red: hsl(0, 100%, 66%);
$White: hsl(0, 0%, 100%);
$Light-grayish-violet: hsl(270, 3%, 87%);
$Dark-grayish-violet: hsl(279, 6%, 55%);
$Very-dark-violet: hsl(278, 68%, 11%);
$Fsize: 18px;
$Font-family: #{$font-stack};

.card {
    grid-row: 1/span 4;
    width: 100%;
    height: 100%;
    background-image: url('./assets/bg-main-desktop.png');
    background-repeat: no-repeat;
    background-size: cover;
    display: grid;
    grid-template-rows: repeat(12, 1fr);
    justify-items: center;
    align-items: center;

    @media (max-width: 70em) {
        grid-row: 1;
        background-image: url('./assets/bg-main-mobile.png');
        grid-template-columns: repeat(3, 1fr);
        grid-template-rows: repeat(8, 1fr);
        height: 15rem;
        width: 100vw;
    }

    & .back__side,
    & .front__side {
        width: 24rem;
        height: 13rem;
        padding: 1.5rem 1.8rem;
        background-repeat: no-repeat;
        background-size: contain;
        cursor: default;
        display: flex;
        flex-direction: column;

        @media (max-width: 27em) {
            width: 18rem;
            height: 10rem;
            padding: 1rem 1.2rem;
        }
    }
}

#app {
    width: 100%;
    height: 100vh;
    display: grid;
    grid-template-columns: minmax(4rem, 1fr) repeat(3, 1fr);
    grid-template-rows: repeat(4, 1fr);

    @media (max-width: 70em) {
        width: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
    }
}

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

html {
    font-family: "Space Grotesk", sans-serif;
    font-size: 112.5%;

    @media (max-width: 90em) {
        font-size: 100%;
    }

    @media (max-width: 70em) {
        font-size: 90%;
    }

    @media (max-width: 22em) {
        font-size: 80%;
    }
}

body {
    height: 100vh;
}

.attribution {
    grid-column: 2/span 3;
    grid-row: 4/5;
    font-size: 11px;
    font-family: inherit;
    text-align: center;
    align-self: end;
    margin-bottom: 0.5rem;

    @media (max-width: 70em) {
        align-self: center;
        margin-bottom: 1rem;
    }

    @media (max-width: 27em) {
        align-self: center;
        height: 1rem;
    }

    a {
        color: hsl(228deg, 45%, 44%);
    }
}

.hidden {
    display: none;
}

.divided {
    width: 45%;
    margin-left: .4rem;
}

.error {
    border: 1px solid red;
}

.success {
    background: linear-gradient(#fff, #fff) padding-box, linear-gradient(to right, rgb(100, 72, 254), rgb(96, 5, 148)) border-box;
    border: 1px solid transparent;
}
</style>
