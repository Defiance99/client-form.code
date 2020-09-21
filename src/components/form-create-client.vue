<template>
    <div>
        <div v-bind:class="{'form-client-container': true, 'correct-form-client': !$v.$invalid}">
            <h2 class="">Персональные данные:</h2>
            <div>
            <p>
                Фамилия
                <span 
                    class="required-simbol"
                    v-bind:class="{correctValueSimbol: !$v.secondName.$invalid, invalidValueSimbol: $v.secondName.$error}"
                >
                    *
                </span>:
            </p>
            <p>
                <input type="text" 
                    placeholder="Иванов"
                    v-model.trim="$v.secondName.$model"
                    @blur="$v.secondName.$touch()"
                    v-bind:class="{correctValue: !$v.secondName.$invalid, invalidValue: $v.secondName.$error}"
                >
                <span class="error-required" v-if="$v.secondName.$error">- Поле 'Фамилия' обязательно</span>
            </p>
            </div>
            <div>
                <p>
                    Имя
                    <span 
                        class="required-simbol"
                        v-bind:class="{correctValueSimbol: !$v.name.$invalid, invalidValueSimbol: $v.name.$error}"
                    >
                        *
                    </span>:
                </p>
                <p>
                    <input type="text" 
                        placeholder="Иван" 
                        v-model.trim="$v.name.$model"
                        @blur="$v.name.$touch()"
                        v-bind:class="{correctValue: !$v.name.$invalid, invalidValue: $v.name.$error}"
                    >
                    <span class="error-required" v-if="$v.name.$error">- Поле 'Имя' обязательно</span>
                </p>
                
            </div>
            <div>
                <p>Отчество:</p>
                <p><input type="text" 
                    placeholder="Иванович"
                    v-bind:class="{correctValue: !!middleName}"
                    v-model="middleName"
                ></p>
            </div>
            <div>
                <p>
                    Дата рождения
                    <span 
                        class="required-simbol" 
                        v-bind:class="{correctValueSimbol: !$v.passportDate.$invalid, invalidValueSimbol: $v.passportDate.$error}"
                    >
                        *
                    </span>
                    :
                </p>
                <p>
                    <input type="date"
                        v-model="$v.passportDate.$model"
                        @blur="$v.passportDate.$touch()"
                        v-bind:class="{correctValue: !$v.passportDate.$invalid, invalidValue: $v.passportDate.$error}"
                    >
                    <span class="error-required" v-if="$v.passportDate.$error">- Укажите дату рождения</span>
                </p>
            </div>
            <div>
                <p>
                    Номер телефона
                    <span 
                        class="required-simbol"
                        v-bind:class="{correctValueSimbol: !$v.numberTel.$invalid, invalidValueSimbol: $v.numberTel.$error}"
                    >
                        *
                    </span>:
                </p>
                <p>
                    <input type="text" placeholder="+7 (___) ___ ____"
                        v-model.trim="$v.numberTel.$model"
                        @blur="$v.numberTel.$touch"
                        ref="numberText"
                        v-on:focus="checkType"
                        v-bind:class="{correctValue: !$v.numberTel.$invalid, invalidValue: $v.numberTel.$error}"
                    >
                    <span class="error-required" v-if="$v.numberTel.$error">- Укажите номер телефона</span>
                </p>
            </div>
            <div>
                <p>Пол:</p>
                <p><input type="text" 
                    placeholder="Мужской"
                    v-bind:class="{correctValue: !!gender}"
                    v-model="gender"
                ></p>
            </div>
            <div class="groups-clients-block">
                <p>
                    Группа клиентов
                    <span 
                        class="required-simbol"
                        v-bind:class="{correctValueSimbol: !$v.groupsOfClients.$invalid, invalidValueSimbol: $v.groupsOfClients.$error}"
                    >
                        *
                    </span>:
                </p>
                <p class="group-clients-container">
                    <ul class="group-clients">
                        <li 
                            v-on:click="showGroupsClients = !showGroupsClients"
                            v-bind:class="{correctValue: !$v.groupsOfClients.$invalid, invalidValue: $v.groupsOfClients.$error}"
                        >
                        <label for="">Отметьте группы:</label>
                        <span class="custom-arrow"></span>
                        </li>
                        <transition-group name="groups-clients" v-if="showGroupsClients">
                            <template>
                                <li key="VIP">
                                    <label for="VIP">VIP</label>
                                    <input type="checkbox" 
                                        id="VIP" 
                                        value="VIP"
                                        @blur="$v.groupsOfClients.$touch()"
                                        v-model="groupsOfClients"
                                    >
                                </li>
                                <li key="problems">
                                    <label for="problems">Проблемные</label>
                                    <input type="checkbox" 
                                        id="problems"
                                        value="problems"
                                        @blur="$v.groupsOfClients.$touch()"
                                        v-model="groupsOfClients"
                                    >
                                </li>
                                <li key="OMS">
                                    <label for="OMS">ОМС</label>
                                    <input type="checkbox" 
                                        id="OMS"
                                        value="OMS"
                                        @blur="$v.groupsOfClients.$touch()"
                                        v-model="groupsOfClients"
                                    >
                                </li>
                            </template>
                        </transition-group>
                        <span class="error-required" v-if="$v.groupsOfClients.$error">- Отметьте группу клиентов</span>
                    </ul>
                </p>
            </div>
            <div>
                <p>Лечащий врач:</p>
                <p>
                    <select v-bind:class="{correctValue: !!doctor}" v-model="doctor">
                        <option>Иванов</option>
                        <option>Захаров</option>
                        <option>Чернышева</option>
                    </select>
                </p>
            </div>
            <div>
                <p><label for="SMS">Не отправлять СМС:</label></p>
                <p><input 
                    type="checkbox" 
                    id="SMS"
                    v-model="putSMS"
                ></p>
            </div>
            <!-- <input type="button" 
                value="Далее" 
                class="button-next" 
                :disabled="$v.$invalid || displayAddressDataBlock"
                v-on:click="displayAddressDataBlock = !displayAddressDataBlock"
            >
            <a href="#address-data">click</a> -->
        </div>
        <transition name="question">
            <div class="modal-window-question" @click="showModalWindowResult" v-if="!$v.$invalid && childFormState">
            <p>Завершить регистрацию?</p>
            <input type="button" value="Завершить">
        </div>
        </transition>
        <transition name="result">  
            <div class="modal-window-result" v-if="isModalWindowR">
                <p>Успешно создано!</p>
            </div>
        </transition>
        <AddressDataComponent v-on:toggleAddressFormState="toggleAddressFormState"></AddressDataComponent>
    </div>
</template>


<script>
import {  required, maxLength } from 'vuelidate/lib/validators'
import AddressDataComponent  from '@/components/address-data'

const regExpNumberTel = (value) => /\+7\s\([0-9]{3}\)\s[0-9]{3}\s[0-9]{4}/.test(value)

export default {
    data() {
        return {
            name: '',
            secondName: '',
            middleName: '',
            passportDate: '',
            numberTel: '',
            gender: '',
            groupsOfClients: [],
            doctor: '',
            putSMS: true,
            showGroupsClients: false,
            childFormState: false,
            isModalWindowR: false
        }
    },
    validations: {
        name: {
            required
        },
        secondName: {
            required
        },
        passportDate: {
            required
        },
        numberTel: {
            required,
            regExpNumberTel,
            maxLength: maxLength(20)
        },
        groupsOfClients: {
            required
        }
    },
    methods: {
        goToAnchor: function(event, href) {
            alert(2)
            event.preventDefault()
            document.querySelector(href).scrollIntoView({
                behavior: 'smooth', block: 'start'
            })
        },
        checkType: function() {
            this.$refs.numberText.addEventListener('keydown', mask, false)
            this.$refs.numberText.addEventListener('blur', mask, false)

            function mask(event) {
                let keyCode;
                event.keyCode && (keyCode = event.keyCode);
                let pos = this.selectionStart;
                if (pos < 3) event.preventDefault();
                
                let matrix = this.placeholder;
                let i = 0;
                let def = matrix.replace(/\D/g, "");
                let val = this.value.replace(/\D/g, "");
                let newValue = matrix.replace(/[_\d]/g, function(a) {
                    return i < val.length ? val.charAt(i++) || def.charAt(i) : a
                });
                i = newValue.indexOf("_");
                if (i != -1) {
                    i < 5 && (i = 3);
                    newValue = newValue.slice(0, i)
                }
                let reg = matrix.substr(0, this.value.length).replace(/_+/g,
                    function(a) {
                        return "\\d{1," + a.length + "}"
                    }).replace(/[+()]/g, "\\$&");
                reg = new RegExp("^" + reg + "$");
                if (!reg.test(this.value) || this.value.length < 5 || keyCode > 47 && keyCode < 58) this.value = newValue;
                if (event.type == "blur" && this.value.length < 5)  this.value = ""
            }
        },
        showModalWindowResult: function() {
            this.childFormState = false
            this.isModalWindowR = true;
            setTimeout(() => {
                this.isModalWindowR = false
            }, 3000)
        },
        toggleAddressFormState: function(value) {
            this.childFormState = value
        }
    },
    components: {
        AddressDataComponent
    }
}

</script>

<style lang="sass">
    .correct-form-client
        border: 1px green solid !important
        box-shadow: 0 0 4px 1px green !important

    .form-client-container 
        display: flex
        flex-direction: column
        justify-content: center
        margin: 0 auto
        width: 420px
        border: 1px black solid
        border-radius: 10px
        box-shadow: 0 0 4px 1px black
    
        div 
            display: flex
            width: 80%
            margin: 0 auto

            p
                width: 42%

                select 
                    width: 177px !important

            p:first-child 
                display: flex
                justify-content: flex-end
                align-items: center

            P:last-child 
                text-align: left

            input 
                height: 20px
            select
                height: 25px
                width: 100px

        .button-next
            margin: 5px auto
            width: 100px
            height: 30px
        .button-next:hover 
            cursor: pointer


    .error-required
        position: absolute
        margin-top: 4px
        color: red
        font-size: 0.8em

    .correctValue 
        border: 2px green solid !important
        border-radius: 5px !important
    .invalidValue 
        border: 2px red solid !important
        border-radius: 5px !important
    .correctValueSimbol    
        color: green
    .invalidValueSimbol  
        color: red

    

    %modal-window-shared
        position: fixed
        left: 5%
        padding: 5px
        border-radius: 3px
        background-color: rgb(50,50,50)
        color: rgb(250,250,250)

    .modal-window-question
        @extend %modal-window-shared
        top: 20%

    .modal-window-result
        @extend %modal-window-shared
        top: 35%
</style>

<style lang="sass" scoped>
    .groups-clients-block
        align-items: flex-start 
        > p
            line-height: 26px

        .group-clients
            margin: 0
            padding: 0

            li
                display: flex
                justify-content: space-between
                position: relative
                margin-top: -1px
                height: 26px
                width: 173px
                border: 1px rgb(118,118,118) solid
                border-radius: 2px

                label
                    font-size: 0.9em
                    width: 98%
                    padding-left: 8px
                    line-height: 26px

    .custom-arrow
        position: absolute
        right: 5px
        top: 7px
        height: 5px
        width: 5px
        padding: 1px
        border-top: 2px black solid
        border-left: 2px black solid
        transform: rotate(225deg)
    
    .custom-arrow:hover, li label:hover
        cursor: pointer
    span li label:hover
        background-color: rgb(30,144,255)
</style>

<style lang="sass" scoped>
    .result-enter-to, .result-leave
        left: 5%
    .result-enter-active, .result-leave-active
        transition: all 1s
    .result-enter, .result-leave-to
        opacity: 0
        left: 0
</style>
