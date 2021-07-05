<template>
    <div class="card">
        <h4 class="card-header text-center">Регистрация нового пациента</h4>
        <div class="card-body">
            <form>
                <div class="form-row row">
                    <h5 class="card-title text-center">Данные о пациенте</h5>
                    <div class="col-sm">
                        <div class="form-group">
                            <label for="">Фамилия*</label>
                            <input type="text" class="form-control" placeholder="Иванов" v-model.trim="$v.lastname.$model" @blur="$v.lastname.$touch()"
                            :class="{'is-invalid':$v.lastname.$error, 'is-valid':!$v.lastname.$invalid }">
                            <div class="valid-feedback"></div>
                            <div class="invalid-feedback">
                                <span v-if="!$v.lastname.required">Это обязательное поле, заполните его. </span>                            
                                <span v-if="!$v.lastname.alpha">Используйте буквы "А-Я". </span>
                                <span v-if="!$v.lastname.minLength">Минимальное количество символов - {{$v.lastname.$params.minLength.min}}. </span>
                                <span v-if="!$v.lastname.maxLength">Максимальное количество символов - {{$v.lastname.$params.maxLength.max}}. </span>
                            </div>


                        <div class="form-group">
                            <label for="">Имя*</label>
                            <input type="text" class="form-control" placeholder="Иван" v-model.trim="$v.firstname.$model" @blur="$v.firstname.$touch()"
                            :class="{'is-invalid':$v.firstname.$error, 'is-valid':!$v.firstname.$invalid}">
                            <div class="invalid-feedback">
                                <span v-if="!$v.firstname.required">Это обязательное поле, заполните его</span>
                                <span v-if="!$v.firstname.alpha">Имя должно состоять из букв</span>
                                <span v-if="!$v.firstname.minLength">Минимальное количество символов - {{$v.firstname.$params.minLength.min}}. </span>
                                <span v-if="!$v.firstname.maxLength">Максимальное количество символов - {{$v.firstname.$params.maxLength.max}}. </span>
                            </div>
                        </div>

                        <div class="form-group">
                            <label for="">Отчество</label>
                            <input type="text" class="form-control" placeholder="Иванович" v-model.trim="$v.surname.$model" :class="{'is-invalid':$v.surname.$error}">
                            <div class="invalid-feedback">                   
                                <span v-if="!$v.surname.alpha">Используйте буквы "А-Я". </span>
                                <span v-if="!$v.surname.minLength">Минимальное количество символов - {{$v.surname.$params.minLength.min}}. </span>
                                <span v-if="!$v.surname.maxLength">Максимальное количество символов - {{$v.surname.$params.maxLength.max}}. </span>
                            </div>
                        </div>

                        <div class="form-group">
                            <label for="">Дата рождения*</label>
                            <input class="form-control" v-model="$v.birthDate.$model" @blur="$v.birthDate.$touch()" type="text" v-imask="dateMask"
                            :class="{'is-invalid':$v.birthDate.$error, 'is-valid':!$v.birthDate.$invalid }" placeholder="01-01-1992" maxlength="10">                      
                            <div class="invalid-feedback">
                                <span v-if="!$v.birthDate.required">Это обязательное поле, заполните его</span>                             
                            </div>
                        </div>

                        <div class="form-group">
                            <label for="">Номер телефона*</label>
                            <input class="form-control" v-model="$v.phoneNumber.$model" @blur="$v.phoneNumber.$touch()" type="text" id="phone" v-imask="phoneNumberMask"
                            :class="{'is-invalid':$v.phoneNumber.$error, 'is-valid':!$v.phoneNumber.$invalid }" placeholder="+7(921)123-45-67"
                            @complete="onComplete" maxlength="16">                     
                            <div class="invalid-feedback">
                            <span v-if="!$v.phoneNumber.required">Это обязательное поле, заполните его</span>
                            </div>
                        </div>
                        
                        <div class="form-group">
                            <div class="form-check form-check-inline">
                                <input class="form-check-input" type="radio" name="inlineRadioOptions" value="Мужской" v-model="gender">
                                <label class="form-check-label" for="inlineRadio1">Мужской</label>
                            </div>
                            <div class="form-check form-check-inline">
                                <input class="form-check-input" type="radio" name="inlineRadioOptions" value="Женский" v-model="gender">
                                <label class="form-check-label" for="inlineRadio2">Женский</label>
                            </div>
                        </div>

                        <div class="form-group">
                            <label for="">Группа клиентов*</label>
                            <select class ="form-select" v-model="$v.clientGroup.$model" @blur="$v.clientGroup.$touch()"
                            :class="{'is-invalid':$v.clientGroup.$error, 'is-valid':!$v.clientGroup.$invalid}">          
                                <option v-for="client in clientGroupList" v-bind:key="client.key">
                                {{ client }}
                                </option>
                            </select>
                            <div class="invalid-feedback">
                                <span v-if="!$v.clientGroup.required">Это обязательное поле, заполните его</span>
                            </div>                       
                        </div>

                        <div class="form-group">
                            <label for="">Лечащий врач</label>
                                <select v-model="doctor" class ="form-select">
                                    <option v-for="doctor in doctorList" v-bind:key="doctor.key">
                                    {{ doctor }}
                                    </option> 
                                </select>               
                        </div>

                        <div class="form-check">
                            <input class="form-check-input" type="checkbox" id="flexCheckDefault" v-model="smsCheck">
                            <label class="form-check-label" for="checkbox">Не отправлять СМС</label>
                        </div>

                    </div>
                </div>
                
                
                <div class="form-row row mt-3">
                    <div class="col-sm">
                        <h5 class="card-title text-center">Адрес</h5>

                        <div class="form-group">
                            <label for="">Индекс</label>
                            <input type="text" class="form-control" placeholder="295000" v-model.trim="$v.index">
                        </div>

                        <div class="form-group">
                            <label for="">Страна</label>
                            <input type="text" class="form-control" placeholder="Россия" v-model.trim="$v.counry">   
                        </div>

                        <div class="form-group">
                            <label for="">Область</label>
                            <input type="text" class="form-control" placeholder="Республика Крым" v-model.trim="$v.region">   
                        </div>

                        <div class="form-group">
                            <label for="">Город*</label>
                            <input type="text" class="form-control" placeholder="Саки" v-model.trim="$v.sity.$model" @blur="$v.sity.$touch()"
                            :class="{'is-invalid':$v.sity.$error, 'is-valid':!$v.sity.$invalid }">
                            <div class="invalid-feedback">
                                <span v-if="!$v.sity.required">Это обязательное поле, заполните его. </span>
                            </div>
                        </div>

                        <div class="form-group">
                            <label for="">Улица</label>
                            <input type="text" class="form-control" placeholder="Ленина" v-model.trim="$v.street">   
                        </div>

                        <div class="form-group">
                            <label for="">Дом</label>
                            <input type="text" class="form-control" placeholder="28" v-model.trim="$v.house">
                        </div>

                    </div>

                    <div class="col-sm">
                        <h5 class="card-title text-center">Данные о документе</h5>

                        <div class="form-group">
                            <label for="">Тип документа*</label>
                            <select class ="form-select" v-model="$v.documentType.$model" @blur="$v.documentType.$touch()"
                            :class="{'is-invalid':$v.documentType.$error, 'is-valid':!$v.documentType.$invalid}">          
                                <option v-for="documentTypeList in documentTypeList" v-bind:key="documentTypeList.key">
                                {{ documentTypeList }}
                                </option>
                            </select>     
                            <div class="invalid-feedback">
                                <span v-if="!$v.documentType.required">Это обязательное поле, заполните его</span>
                            </div>
                        </div>

                        <div class="form-group">
                            <label for="">Серия</label>
                            <input type="text" class="form-control" placeholder="3216" v-model.trim="documentSerialNumber">
                        </div>

                        <div class="form-group">
                            <label for="">Номер</label>
                            <input type="text" class="form-control" placeholder="543687" v-model.trim="documentNumber">
                        </div>

                        <div class="form-group">
                            <label for="">Кем выдан</label>
                            <input type="text" class="form-control" placeholder="ФМС по г. Саки 914-003" v-model.trim="documentReliser">
                        </div>

                        <div class="form-group">
                            <label for="">Дата выдачи*</label>
                            <input type="text" class="form-control" placeholder="01-01-1992"  v-model="$v.documentReliseTime.$model" @blur="$v.documentReliseTime.$touch()" v-imask="dateMask"
                            :class="{'is-invalid':$v.documentReliseTime.$error, 'is-valid':!$v.documentReliseTime.$invalid }" @keypress="isNumber" maxlength="10"  >                      
                            <div class="invalid-feedback">
                                <span v-if="!$v.documentReliseTime.required">Это обязательное поле, заполните его</span>                                  
                            </div>
                        </div>
                        
                      
                    </div>
                    </div>
                </div>
                <div class="form-row row">
                    <div class="col">
                        <button type="button" class="btn btn-success" :disabled="$v.$invalid && $v.$error" @click="onSubmit">Подать заявку</button>
                    </div> 

                </div>
            </form>
        </div>
    </div>
</template>

<script>
import { required, minLength, maxLength } from 'vuelidate/lib/validators'
import {IMaskDirective} from 'vue-imask';
export default {
    name: 'FormValidation',
    components: {
        
    },
    data () {
        return {
            firstname: '',
            lastname: '',
            surname: '',
            birthDate: '',
            phoneNumber: '',
            gender: '',
            clientGroup: '',
            clientGroupList: [
                "VIP",
                "Проблемные",
                "ОМС"
            ],
            doctor: '',
            doctorList: [
                "Иванов",
                "Захаров",
                "Чернышева",
                    ],
            smsCheck: false,
            index: '',
            counry: '',
            region: '',
            sity: '',
            street: '',
            house: '',
            documentType: '',
            documentTypeList: [
                "Паспорт",
                "Свидетельство о рождении",
                "Водительское удорстоверение"
            ],
            documentSerialNumber: '',
            documentNumber: '',
            documentReliser: '',
            documentReliseTime: '',
            phoneNumberMask: {
                mask: '+{7}(000)000-00-00',
    },
            dateMask: {
                mask: '00-00-0000'
    },
    
            }
    },
    directives: {
  imask: IMaskDirective
},
    
    validations: {
        firstname: {
            required,
            alpha:  value => /^[а-яёa-z]*$/i.test(value),
            minLength: minLength(3),
            maxLength: maxLength(10)
            },
            lastname: {
                required,
                alpha:  value => /^[а-яёa-z]*$/i.test(value),
                minLength: minLength(3),
                maxLength: maxLength(15),
            },
            surname: {
                alpha:  value => /^[а-яёa-z]*$/i.test(value),
                minLength: minLength(3),
                maxLength: maxLength(15)
            },
            birthDate: {
                  required,

            },
            phoneNumber: {
                required,
                },
            clientGroup: {
                required
                },
            sity: {
                required
                },
            documentType: {
                required
                },
            documentReliseTime: {
                required,
                minLength: minLength(10),
                },
        },
         methods: {
    onSubmit() {
        this.$v.$touch();
        if (!this.$v.invalid) {
            alert(this.firstname)
        }
    },
    onComplete(e) {
        const maskRef = e.detail
        this.phoneNumber = maskRef.unmaskedValue
    },
    isNumber(e) {
        let regex = /[0-9]/

        if (!regex.test(e.key)) {
        e.returnValue = false;
        if (e.preventDefault) e.preventDefault();
        }
    },
    }
    }
</script>
<style scoped>
</style>