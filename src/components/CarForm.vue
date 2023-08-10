<template>
    <div class="car-form">
        <simple-card class="vd-form">
            <template #title>Car Comparison</template>

            <template #content class="info">
                <p>
                    Please enter information
                </p>
                <form>
                    <label>License Plate</label>
                    <input type="text" v-model="LicensePlate" @keypress="checkCapsLP('lox')" @keypress.enter="whatCar">
                    <span v-if="checkLP" class="error"> {{ error }} </span>
                    <label>Zipcode</label>
                    <div class="zip">
                        <input type="text" v-model="Zipcode.letters" v-model.trim="Zipcode.letters"
                            @keypress="checkZipcode($event)" @keydown="checkCapsZip">
                        <input type="number" v-model="Zipcode.numbers" v-model.trim="Zipcode.numbers">
                    </div>
                    <span v-if="checkZip" class="error"> {{ error }} </span>
                    <label>House Number</label>
                    <input type="number" v-model="Housenumber" required>
                    <label>House Number Addition</label>
                    <input type="text" v-model="HousenumberAddition">
                    <label>Birthdate</label>
                    <div class="BD">
                        <input type="number" v-model="birthdate.day" placeholder="DD" @keypress="checkDate"><span>/</span>
                        <input type="number" v-model="birthdate.month" placeholder="MM" @keypress="checkDate"><span>/</span>
                        <input type="number" v-model="birthdate.year" placeholder="YYYY" @keypress="checkDate">
                    </div>
                    <span v-if="checkBD" class="error"> {{ error }} </span>
                    <label>Claim Free Years</label>
                    <select v-model="ClaimFree">
                        <option value="0-5">0-5</option>
                        <option value="5-10">5-10</option>
                        <option value="10-15">10-15</option>
                        <option value="15-20">15-20</option>
                        <option value="20-25">20-25</option>
                    </select>
                    <label>Kilometrage</label>
                    <select v-model="Kilometrage">
                        <option value="0 t/m 7500 KM">0 t/m 7500 KM</option>
                        <option value="7501 t/m 10000 KM">7501 t/m 10000 KM</option>
                        <option value="10001 t/m 12000 KM">10001 t/m 12000 KM</option>
                        <option value="12001 t/m 15000 KM">12001 t/m 15000 KM</option>
                        <option value="15001 t/m 20000 KM">7501 t/m 10000 KM</option>
                        <option value="20001 t/m 25000 KM">7501 t/m 10000 KM</option>
                        <option value="30001 t/m 90000 KM">7501 t/m 10000 KM</option>
                    </select>
                </form>
                <div class="btn" @click="onSubmit">
                    Confirm
                </div>
            </template>
            <template #smallCard>some text</template>
        </simple-card>
        <SmallCard v-if="showCard" class="small-card">
            <h1> {{ LP }} </h1>
            <h3> {{ brand }} </h3>
            <h3> {{ year }} </h3>
        </SmallCard>
    </div>
</template>

<script>
import { ref } from '@vue/reactivity';
import SimpleCard from "./SimpleCard.vue";
import SmallCard from "./SmallCard.vue";

export default {
    components: {
        SimpleCard,
        SmallCard
    },
    setup() {
        const LicensePlate = ref("") //5PE2Z7, 13ABX2, 66SAG0
        const cars = ref([])
        const LP = ref("")
        const brand = ref("")
        const year = ref(0)
        const Zipcode = ref([{ letters: "", numbers: 0 }])
        const Housenumber = ref(null)
        const HousenumberAddition = ref("")
        const birthdate = ref([{ day: 0, month: 0, year: 0 }])
        const ClaimFree = ref("0-5")
        const Kilometrage = ref("7501 t/m 10000 KM")
        const error = ref("")
        let checkLP = ref(false)
        let checkZip = ref(false)
        let checkBD = ref(false)
        let showCard = ref(false)

        const checkCapsLP = (str) => {
            if (LicensePlate.value === LicensePlate.value.toUpperCase()) {
                checkLP.value = false
            } else {
                error.value = str.value
                checkLP.value = true
            }
        }
        const checkCapsZip = (word) => {
            if (word.target.value === word.target.value.toUpperCase()) {
                checkZip.value = false
            } else {
                error.value = "Enter only upercase"
                checkZip.value = true
            }
        }

        const checkZipcode = (e) => {
            if (Zipcode.value.letters === false) {
                check.value = true
            }

            let char = String.fromCharCode(e.keyCode)
            if (/^[A-Za-z]+$/.test(char)) return true;
            else e.preventDefault();
        }

        const whatCar = () => {
            for (const key in cars.value) {
                if (cars.value[key].LP === LicensePlate.value) {
                    LP.value = cars.value[key].LP
                    brand.value = cars.value[key].brand
                    year.value = cars.value[key].year
                    showCard.value = true
                }
            }


        }
        const checkDate = () => {
            if (birthdate.value.day > 31 ||
                birthdate.value.month > 12 ||
                birthdate.value.year > 2023
            ) {
                error.value = "Date is incorect"
                checkBD.value = true
            }
            else {
                checkBD.value = false
            }
        }

        const load = async () => {
            try {
                let data = await fetch("http://localhost:3000/cars")
                if (!data.ok) {
                    throw Error("no data available")
                }
                cars.value = await data.json()
            } catch (err) {
                error.value = err.message
                console.log(error.value);
            }
        }
        load()

        const onSubmit = () => {
            console.log(
                'LicensePlate: ', LicensePlate.value,
                'Zipcode: ', Zipcode.value.letters + Zipcode.value.numbers,
                'Housenumber: ', Housenumber.value,
                'HousenumberAddition: ', HousenumberAddition.value,
                'birthdate day: ', birthdate.value.day,
                'birthdate month: ', birthdate.value.month,
                'birthdate year: ', birthdate.value.year,
                'ClaimFree: ', ClaimFree.value,
                'Kilometrage: ', Kilometrage.value,
            );
        }

        return {
            LicensePlate,
            checkLP,
            checkZip,
            checkBD,
            cars,
            LP,
            brand,
            year,
            showCard,
            Zipcode,
            Housenumber,
            HousenumberAddition,
            birthdate,
            ClaimFree,
            Kilometrage,
            error,
            checkCapsLP,
            checkCapsZip,
            checkZipcode,
            whatCar,
            checkDate,
            onSubmit
        }
    }
}
</script>

<style scoped>
.car-form {
    position: relative;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    justify-content: space-between;
    max-width: 430px;
    margin: auto;
}

.small-card {
    position: absolute;
    margin-top: 162px;
    right: -270px;
}

.vd-form {
    width: 430px;
    height: 730px;
    background-color: rgba(255, 255, 255, 0.95);
}

@supports (-webkit-backdrop-filter: none) or (backdrop-filter: none) {
    .vd-form {
        -webkit-backdrop-filter: blur(1px);
        backdrop-filter: blur(1px);
        background-color: rgba(255, 255, 255, 0.5);
    }
}

@media only screen and (max-width: 768px) {
    .vd-form {
        width: 100%;
    }
}

.btn {
    margin-top: 18px;
    background: #4bb4fc;
    text-align: center;
    padding: 10px 10px;
    font-weight: 600;
    color: white;
    border-radius: 12px;
    cursor: pointer;
    transition: .1s ease;
}

.btn:hover {
    background: #7cc4f3;
}

.btn:focus {
    background-color: #497491;
    border-radius: 12px;
}

form {
    display: flex;
    flex-direction: column;
    text-align: left;
    background-color: #fff;
    border-radius: 20px;
    font-size: 18px;
    font-weight: 600;
    padding: 24px;
}

select {
    height: 35px;
    border: none;
    background-color: #cdebfd;
    border-radius: 5px;
    font-size: 16px;
    margin-top: 4px;
    padding-left: 5px;
}

select:focus {
    outline: none;
}

input {
    height: 25px;
    border: none;
    background-color: #cdebfd;
    border-radius: 5px;
    font-size: 16px;
    margin-top: 4px;
    padding: 4px;
    padding-left: 6px;
}

input:focus {
    outline: none;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
}

.BD {
    display: flex;
    justify-content: space-around;
    width: 170px;
    margin-left: -3px;
    align-items: center;
}

.BD span {
    margin-top: 5px;
    color: #929292;
}

.BD input {
    width: 36px;
    text-align: center;
    padding-left: 0;
}

.BD input:last-child {
    width: 46px;
    padding-left: 3px;
}

.zip {
    display: flex;
    width: 92px;
    justify-content: space-between;
}

.zip input {
    width: 48px;
}

.zip input:last-child {
    width: 20px;
}

label {
    margin-top: 12px;
}

.error {
    color: #f06d6d;
    font-size: 14px;
    font-weight: 200;
}
</style>
