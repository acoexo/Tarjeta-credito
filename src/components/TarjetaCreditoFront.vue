<!-- Componente anverso de la tarjeta -->
<template>
  <div class="front__side">
    <div class="front__logo">
      <div class="front__logo--1">&nbsp;</div>
      <div class="front__logo--2">&nbsp;</div>
    </div>
    <div class="front__info">
      <h2 class="front__number">{{ cardNumber ? formatCardNumber(cardNumber) : '0000 0000 0000 0000' }}</h2>
      <div class="front__info--bottom">
        <p class="front__person">{{ holderName ? holderName : 'Jane Appleseed' }}</p>
        <p class="front__date">
          <span class="front__date--month">{{ expDateMonth ? expDateMonth : '00' }}</span>/<span
            class="front__date--year">{{ expDateYear ? expDateYear : '00' }}</span>
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps } from 'vue';

// Definir las propiedades esperadas por el componente
const props = defineProps({
  cardNumber: String,
  holderName: String,
  expDateMonth: String,
  expDateYear: String
});

// Función para formatear el número de la tarjeta dividiéndolo en grupos de cuatro caracteres
const formatCardNumber = (number) => {
  // Eliminar cualquier espacio en blanco presente en el número
  number = number.replace(/\s/g, "");

  // Dividir el número en grupos de cuatro caracteres
  const parts = number.match(/.{1,4}/g);

  // Unir los grupos con espacios entre ellos
  const formattedNumber = parts.join(" ");

  return formattedNumber;
};
</script>

<style lang="scss">
.front__side {
  grid-row: 3/6;
  background-image: url('../assets/bg-card-front.png');
  justify-content: space-between;
  transform: translate(25%, 10%);

  @media (max-width: 70em) {
    grid-row: 5/span 5;
    grid-column: 2/3;
    transform: translate(-10%, 10%);
    z-index: 10;
  }

  @media (max-width: 22em) {
    grid-column: 2/3;
    transform: translateX(-10%);
  }
}

.front__logo {
  display: flex;
  align-items: center;
}

.front__logo--1 {
  width: 2rem;
  height: 2rem;
  background-color: hsl(0deg, 0%, 100%);
  border-radius: 50%;

  @media (max-width: 70em) {
    width: 1.5rem;
    height: 1.5rem;
  }
}

.front__logo--2 {
  width: 1rem;
  height: 1rem;
  border: 1px solid hsl(0deg, 0%, 100%);
  border-radius: 50%;
  margin-left: 1rem;

  @media (max-width: 70em) {
    width: 0.8rem;
    height: 0.8rem;
  }
}


.front__info {
  width: 100%;
  height: 40%;
  color: hsl(0deg, 0%, 100%);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.front__info--bottom {
  font-size: 0.75rem;
  letter-spacing: 2px;
  display: flex;
  justify-content: space-between;

  @media (max-width: 27em) {
    font-size: 0.7rem;
  }
}

.front__number {
  font-size: 1.2rem;
  font-weight: 500;
  letter-spacing: 4px;
  word-spacing: 5px;

  @media (max-width: 27em) {
    font-size: 0.9rem;
    letter-spacing: 3px;
    word-spacing: 2px;
  }
}
</style>