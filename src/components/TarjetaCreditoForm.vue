<template>
    <!-- Formulario de ingreso de datos de la tarjeta de crédito -->
    <form action="#" class="form" @submit.prevent="submitForm">
        <div class="wrapper">
            <!-- Campo para el nombre del titular de la tarjeta -->
            <div class="form__item form__item--1 mb-md">
                <label class="mb-sm" for="name">Cardholder Name</label>
                <input type="text" name="name" id="name" class="form__input" autocomplete="cc-csc"
                    placeholder="e.g. Asliddin Amirov" v-model="holderName" maxlength="20"
                    @click="hideErrorMessage('name')">
                <p class="invalid invalid--1" v-if="nameErrorMessage">{{ nameErrorMessage }}</p>
            </div>
            <!-- Campo para el número de la tarjeta -->
            <div class="form__item form__item--2 mb-md">
                <label class="mb-sm" for="number">Card Number</label>
                <input type="text" name="number" id="number" class="form__input" autocomplete="cc-csc"
                    placeholder="e.g. 1234 5678 9123 0000" v-model="cardNumber" maxlength="16"
                    @click="hideErrorMessage('number')">
                <p class="invalid invalid--2" v-if="numberErrorMessage">{{ numberErrorMessage }}</p>
            </div>
            <!-- Campos para el mes y el año de vencimiento -->
            <div class="form__row mb-md">
                <div class="form__item form__item-3">
                    <label class="mb-sm" for="date">Exp. Date (MM/YY)</label>
                    <div>
                        <input class="form__input divided" type="text" name="date_month" id="date" autocomplete="cc-csc"
                            placeholder="MM" v-model="expDateMonth" maxlength="2"
                            @click="hideErrorMessage(dateErrorMessage.value)">
                        <input class="form__input divided" type="text" name="date_year" id="date" autocomplete="cc-csc"
                            placeholder="YY" v-model="expDateYear" maxlength="2"
                            @click="hideErrorMessage('date')">
                    </div>
                    <p class="invalid invalid--3" v-if="dateErrorMessage">{{ dateErrorMessage }}</p>
                </div>
                <!-- Campo para el CVC -->
                <div class="form__item form__item--4">
                    <label class="mb-sm" for="pin">CVC</label>
                    <input type="text" name="pin" id="pin" class="form__input" autocomplete="cc-csc"
                        placeholder="e.g. 123" v-model="cardCVC" maxlength="3"
                        @click="hideErrorMessage('cvc')">
                    <p class="invalid invalid--4" v-if="cvcErrorMessage">{{ cvcErrorMessage }}</p>
                </div>
            </div>
            <!-- Botón de confirmación -->
            <button class="button" id="btn-submit">Confirm</button>
        </div>
        <!-- Mensaje de confirmación -->
        <div class="complete hidden">
            <div class="complete__icon">
                <svg width="80" height="80" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <circle cx="40" cy="40" r="40" fill="url(#a)" />
                    <path d="M28 39.92 36.08 48l16-16" stroke="#fff" stroke-width="3" />
                    <defs>
                        <linearGradient id="a" x1="-23.014" y1="11.507" x2="0" y2="91.507"
                            gradientUnits="userSpaceOnUse">
                            <stop stop-color="#6348FE" />
                            <stop offset="1" stop-color="#610595" />
                        </linearGradient>
                    </defs>
                </svg>
            </div>
            <div class="complete__text">
                <h3>Thank You!</h3>
                <p>We've added your card details</p>
            </div>
        </div>
    </form>
</template>

<script setup>
import { ref, defineEmits } from 'vue';

// Definimos los eventos que este componente puede emitir
const emit = defineEmits(['formSubmitted']);

// Utilizamos ref para crear variables reactivas
const cardNumber = ref("");
const holderName = ref("");
const expDateMonth = ref("");
const expDateYear = ref("");
const cardCVC = ref("");

// Definimos las referencias reactivas para los mensajes de error
const nameErrorMessage = ref("");
const numberErrorMessage = ref("");
const dateErrorMessage = ref("");
const cvcErrorMessage = ref("");

// Funciones de validación
const isNameValid = () => {
    const namePattern = /^[A-Za-záéíóúÁÉÍÓÚüÜñÑ.]+$/;
    if (!holderName.value.trim()) {
        return "Name cannot be empty";
    }
    if (!namePattern.test(holderName.value)) {
        return "Name can only contain letters with accents and dots";
    }
    return true;
};

const isCardNumberValid = () => {
    if (!cardNumber.value.trim()) {
        return "Card number cannot be empty";
    }
    return /^(\d\s?){16}$/.test(cardNumber.value) || "Card number must have 16 digits";
};

const isDateValid = () => {
    const currentDate = new Date();
    const currentYear = currentDate.getFullYear() % 100;
    if (!expDateMonth.value.trim() || !expDateYear.value.trim()) {
        return "Expiration date cannot be empty";
    }
    if (expDateYear.value < currentYear || (expDateMonth.value > 12 || expDateMonth.value < 1)) {
        return "Invalid date"
    }
    return /^(\d{2})\/(\d{2})$/.test(expDateMonth.value + '/' + expDateYear.value) || "Invalid date format (MM/YY)";
};

const isCardCVCValid = () => {
    if (!cardCVC.value.trim()) {
        return "CVC cannot be empty";
    }
    return /^\d{3}$/.test(cardCVC.value) || "CVC must have 3 digits";
};


// Función que oculta los mensajes de error
const hideErrorMessage = (error = "all") => {
    if (error === "all") {
        numberErrorMessage.value = "";
        nameErrorMessage.value = "";
        cvcErrorMessage.value = "";
        dateErrorMessage.value = "";
    } else {
        switch (error) {
            case "name":
                nameErrorMessage.value = "";
                break;
            case "number":
                numberErrorMessage.value = "";
                break;
            case "date":
                dateErrorMessage.value = "";
                break;
            case "cvc":
                cvcErrorMessage.value = "";
                break;
            default:
                break;
        }
    }
};


// Función para enviar el formulario
const submitForm = () => {
    const nameValidation = isNameValid();
    const numberValidation = isCardNumberValid();
    const dateValidation = isDateValid();
    const cvcValidation = isCardCVCValid();

    if (nameValidation === true && numberValidation === true && dateValidation === true && cvcValidation === true) {
        const formData = {
            cardNumber: cardNumber.value,
            holderName: holderName.value,
            expDateMonth: expDateMonth.value,
            expDateYear: expDateYear.value,
            cardCVC: cardCVC.value
        };

        // Emitimos el evento 'formSubmitted' con los datos del formulario
        emit('formSubmitted', formData);

        // Ocultamos el formulario y mostramos un mensaje de éxito temporal
        document.querySelector(".wrapper").classList.add("hidden");
        document.querySelector(".complete").classList.remove("hidden");

        setTimeout(() => {
            cardNumber.value = "";
            holderName.value = "";
            cardCVC.value = "";
            expDateMonth.value = "";
            expDateYear.value = "";

            document.querySelector(".wrapper").classList.remove("hidden");
            document.querySelector(".complete").classList.add("hidden");
        }, 3000);
    }
    else {
        // Actualizamos los mensajes de error correspondientes
        nameErrorMessage.value = nameValidation !== true ? nameValidation : "";
        numberErrorMessage.value = numberValidation !== true ? numberValidation : "";
        dateErrorMessage.value = dateValidation !== true ? dateValidation : "";
        cvcErrorMessage.value = cvcValidation !== true ? cvcValidation : "";

        setTimeout(() => {
            hideErrorMessage("all");
            document.querySelector(".wrapper").classList.remove("hidden");
            document.querySelector(".complete").classList.add("hidden");
        }, 3000);
    }
};

</script>


<style lang="scss">
$font-stack: "Space Grotesk", sans-serif;

$linear-gradient: linear-gradient(hsl(249, 99%, 64%) to hsl(278, 94%, 30%));
$red: hsl(0, 100%, 66%);
$White: hsl(0, 0%, 100%);
$Light-grayish-violet: rgb(75, 21, 106);
$Dark-grayish-violet: hsl(279, 6%, 55%);
$Very-dark-violet: hsl(278, 68%, 11%);
$Fsize: 18px;
$Font-family: #{$font-stack};

.form {
    grid-row: 2/span 2;
    grid-column: 3/span 1;
    width: 100%;
    max-width: 30rem;
    min-height: 25rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    letter-spacing: 1px;
}

label {
    color: $Very-dark-violet;
    font-size: 0.8rem;
    font-weight: 500;
    text-transform: uppercase;
}

.button {
    width: 100%;
    color: $White;
    background-color: $Very-dark-violet;
    border: none;
    outline: none;
    border-radius: 0.5rem;
    height: 2.5rem;
    margin-top: 0.5rem;
    transition: all 0.1s;
    font-family: inherit;

    :hover {
        background-color: $Light-grayish-violet;
    }

    :active {
        background-color: $Dark-grayish-violet;
    }
}

@media (max-width: 70em) {
    .form {
        justify-content: start;
        flex: 1;
        width: 90%;
        height: 22rem;
        margin-top: 10rem;
    }
}

@media (max-width: 70em) and (orientation: landscape) {
    .form {
        margin-top: 10rem;
        margin-bottom: 2rem;
    }
}

@media (max-width: 27em) {
    .form {
        margin-top: 3rem;
        margin-bottom: -2rem;
    }
}





.form__input {
    outline: none;
    border: 1px solid $Light-grayish-violet;
    height: 2.4rem;
    border-radius: 0.5rem;
    padding: 1rem 0.8rem;
    font-size: 0.8rem;
    margin-bottom: 5px;

    ::-webkit-outer-spin-button,
    ::-webkit-inner-spin-button {
        -webkit-appearance: none;
        margin: 0;
    }

    :active,
    :focus {
        background: linear-gradient(#fff, #fff) padding-box, linear-gradient(to right, rgb(100, 72, 254), rgb(96, 5, 148)) border-box;
        border: 1px solid transparent;
        border-radius: 0.5rem;
        padding-left: 1.2rem;
    }

    ::-moz-placeholder {
        color: rgba(142, 133, 147, 0.7);
        font-family: "Space Grotesk", sans-serif;
    }

    :-ms-input-placeholder {
        color: rgba(142, 133, 147, 0.7);
        font-family: "Space Grotesk", sans-serif;
    }

    ::placeholder {
        color: rgba(142, 133, 147, 0.7);
        font-family: "Space Grotesk", sans-serif;
    }
}

.form__item {
    display: flex;
    flex-direction: column;
}

.form__item--1,
.form__item--2 {
    width: 100%;
    align-self: start;
}

.form__item--4 {
    width: 40%;
}

.form__row {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.complete {
    width: 100%;
    height: 80%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-evenly;
}

.complete__text {
    text-align: center;
    letter-spacing: 3px;

    h3 {
        color: $Very-dark-violet;
        font-size: 1.2rem;
        text-transform: uppercase;
        margin-bottom: 1rem;

        @media (max-width: 27em) {
            font-size: 1.5rem;
        }
    }
}

.complete__text p {
    color: hsl(279deg, 6%, 55%);
    letter-spacing: 0;
    font-size: 1rem;
}

.invalid {
    display: block;
    height: 1rem;
    color: red;
    font-size: 0.8rem;
    letter-spacing: 0;
    padding-left: 8px;
}

.mb-sm {
    margin-bottom: 0.325rem;
}

.mb-md {
    margin-bottom: 1rem;
}
</style>
