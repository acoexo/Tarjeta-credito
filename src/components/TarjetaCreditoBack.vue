<!-- Componente reverso de la tarjeta -->
<template>
    <div class="back__side">
        <p class="back__pin">{{ cardCVC ? cardCVC : '000' }}</p>
    </div>
</template>

<script setup>
import { defineProps } from 'vue';

// Definir las propiedades esperadas por el componente
const props = defineProps({
    cardCVC: String 
});
</script>
<style lang="scss">
$font-stack: "Space Grotesk", sans-serif;

$linear-gradient: linear-gradient(hsl(249, 99%, 64%) to hsl(278, 94%, 30%));
$red: hsl(0, 100%, 66%);
$White: hsl(0, 0%, 100%);
$Light-grayish-violet: hsl(270, 3%, 87%);
$Dark-grayish-violet: hsl(279, 6%, 55%);
$Very-dark-violet: hsl(278, 68%, 11%);
$Fsize: 18px;
$Font-family: #{$font-stack};

.back__side {
    grid-row: 8/span 2;
    transform: translateX(40%);
    background-image: url('../assets/bg-card-back.png');
    align-items: flex-end;
    justify-content: center;

    @media (max-width: 70em) {
        grid-row: 2/span 5;
        grid-column: 2/3;
        transform: translate(10%, 10%);
    }

    @media (max-width: 22em) {
        grid-column: 2/3;
        transform: translate(10%);
    }
}


.back__pin {
    color: hsl(0deg, 0%, 100%);
    letter-spacing: 1px;
    margin-right: 1.5rem;

    @media (max-width: 70em) {
        margin-right: 1rem;
    }
}
</style>