<template>
    <div v-bind:class="{'form-client-container': true, 'correct-form-client': !$v.$invalid}">
        <h2>Паспротные данные:</h2>
        <div>
            <p>
                Тип документа
                <span v-bind:class="{correctValueSimbol: !$v.document.$invalid, invalidValueSimbol: $v.document.$error}"> 
                    *
                </span>
                : 
            </p>
            <p>
                <select 
                    v-bind:class="{correctValue: !$v.document.$invalid, invalidValue: $v.document.$error}" 
                    v-model.lazy="document"
                    @blur="$v.document.$touch()"
                >
                    <option>Паспорт</option>
                    <option>Свидетельство о рождении</option>
                    <option>Вод. удостоверение</option>
                </select>
                <span class="error-required" v-if="$v.document.$error">- Отметьте тип документа:</span>
            </p>
        </div>
        <div>
            <p>
                Серия:
            </p>
            <p>
                <input type="text" placeholder="" v-model="series" v-bind:class="{correctValue: !!series}">
            </p>
        </div>
        <div>
            <p>
                Номер:
            </p>
            <p>
                <input type="text" placeholder="" v-model="docNumber" v-bind:class="{correctValue: !!docNumber}">
            </p>
        </div>
        <div>
            <p>
                Кем выдан:
            </p>
            <p>
                <input type="text" v-model="placeOfIssue" v-bind:class="{correctValue: !!placeOfIssue}">
            </p>
        </div>
        <div>
            <p>
                Дата выдачи
                <span class="required-simbol" v-bind:class="{correctValueSimbol: !$v.dateOfIssue.$invalid, invalidValueSimbol: $v.dateOfIssue.$error}">
                    *
                </span>
                :
            </p>
            <p>
                <input type="date" 
                    v-model.lazy="dateOfIssue" 
                    @blur="$v.dateOfIssue.$touch()"

                    v-bind:class="{correctValue: !$v.dateOfIssue.$invalid, invalidValue: $v.dateOfIssue.$error}"
                >
                <span class="error-required" v-if="$v.dateOfIssue.$error">- Укажите дату выдачи:</span>
            </p>
        </div>
    </div>
</template>

<script>
import {  required } from 'vuelidate/lib/validators'

export default {
    data() {
        return {
            document: '',
            series: '',
            docNumber: '',
            placeOfIssue: '',
            dateOfIssue: '',
        }
    },
    validations: {
        document: {
            required
        },
        dateOfIssue: {
            required
        }
    },
    watch: {
        document: function() {
            if (!!this.document && !!this.dateOfIssue) this.$emit('togglePassportState', true)
            else this.$emit('togglePassportState', false)
        },
        dateOfIssue: function() {
            if (!!this.document && !!this.dateOfIssue) this.$emit('togglePassportState', true)
            else this.$emit('togglePassportState', false)
        }
    }
}
</script>

<style lang="sass" scoped>
    .form-client-container
        padding-bottom: 10px

    
</style>