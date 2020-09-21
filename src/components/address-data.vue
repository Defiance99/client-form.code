<template>
    <div>
        <div v-bind:class="{'form-client-container': true, 'correct-form-client': !$v.$invalid}">
            <h2>Адресные данные:</h2>
            <div class="countries-container">
                <p>
                    Страна:
                </p>
                <p>
                    <input type="text"
                        placeholder="Россия"
                        v-model="country"
                        @focus="getCountries"
                        v-bind:class="{correctValue: !!country}"
                        list="list-of-countries"
                    >
                    <datalist id="list-of-countries">
                        <option v-for="country of countries" v-bind:key="country.id">{{country.name}}</option>
                    </datalist>
                    
                </p>
            </div>
            <div>
                <p>
                    Область:
                </p>
                <p>
                    <input type="text"
                        placeholder="Амурская"
                        v-model="region"
                        v-bind:class="{correctValue: !!region}"
                    >
                </p>
            </div>
            <div>
                <p>
                    Город
                    <span v-bind:class="{correctValueSimbol: !$v.city.$invalid, invalidValueSimbol: $v.city.$error}">
                    *
                    </span>
                    :
                </p>
                <p>
                    <input type="text"
                        placeholder="Белогороск"
                        v-model="city"
                        @blur="$v.city.$touch()"
                        v-bind:class="{correctValue: !$v.city.$invalid, invalidValue: $v.city.$error}"
                        list="list-of-cities"
                    >
                    <span class="error-required" v-if="$v.city.$error">- Поле "Город" обязательно</span>
                    <datalist id="list-of-cities">
                        <option v-for="city of cities" v-bind:key="city.id * Math.random()">{{city.name}}</option>
                    </datalist>
                </p>
                
            </div>
            <div>
                <p>
                    Улица:
                </p>
                <p>
                    <input type="text"
                        placeholder="Победная"
                        v-model="street"
                        v-bind:class="{correctValue: !!street}"
                    >
                </p>
            </div>
            <div>
                <p>
                    Дом:
                </p>
                <p>
                    <input type="text"
                        placeholder="Победный"
                        v-model="house"
                        v-bind:class="{correctValue: !!house}"
                    >
                </p>
            </div>
            <div>
                <p>
                    Индекс:
                </p>
                <p>
                    <input type="text"
                        placeholder="1"
                        v-model="index"
                        v-bind:class="{correctValue: !!index}"
                    >
                </p>
            </div>
        </div>
        <PassportDataComponent v-on:togglePassportState="togglePassportState"></PassportDataComponent>
    </div>
</template>

<script>
import {  required } from 'vuelidate/lib/validators'
import { token } from '@/config/keys'
import PassportDataComponent from '@/components/passport-data'

export default {
    data() {
        return {
            index: '',
            country: '', 
            region: '',
            city: '',
            street: '',
            house: '',
            query: 'Россия',
            countries: [],
            token: '',
            passportFormState: false,
        }
    },
    validations: {
        city: {
            required
        },
    },
    methods: {
        getCountries: function() {
            if (this.countries != '') return
            let url = `http://geohelper.info/api/v1/countries?apiKey=${token}&locale%5Blang%5D=ru`
            
            fetch(url)
                .then(response => response.text())
                .then(result => {
                    for (let country of JSON.parse(result).result) {
                        this.countries.push({id: country.id, name: country.name, iso: country.iso});
                    }
                })
                .catch(error => console.log("error", error));
            
        },
        togglePassportState: function(value) {
            this.passportFormState = value
        }
    },
    watch: {
        city: function() {
            if (!!this.city && this.passportFormState) this.$emit('toggleAddressFormState', true)
            else this.$emit('toggleAddressFormState', false)
        } 
    },
    components: {
        PassportDataComponent
    }
}

</script>

<style lang="sass" scoped>
    .form-client-container
        margin-top: 15px
    .countries-container
        position: relative
        
    #list-of-countries 
        height: 200px !important
        
</style>