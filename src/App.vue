<template>
  <div id="container">
    <form @submit.prevent="someAction()">
      <div class="field">
        <label for="name">Имя</label>
        <input id="name" type="text" v-model="name" @blur="$v.name.$touch()" />
        <div class="error" v-if="$v.name.$error">
          <template
            v-if="!$v.name.maxLength"
          >Длина имени не должна превышать {{ $v.name.$params.maxLength.max }} символов</template>
          <template v-else-if="!$v.name.alpha">Имя должно содержать только буквы кириллицы</template>
          <template v-else>Имя обязательно для заполнения</template>
        </div>
      </div>

      <div class="field">
        <label for="last_name">Фамилия</label>
        <input id="last_name" type="text" v-model="lastName" @blur="$v.lastName.$touch()" />
        <div class="error" v-if="$v.lastName.$error">
          <template
            v-if="!$v.lastName.minLength"
          >Длинна фамилии не должна быть меньше {{ $v.lastname.$params.minLength.min }} символов</template>
          <template
            v-else-if="!$v.lastName.maxLength"
          >Длина фамилии не должна превышать {{ $v.lastName.$params.maxLength.max }} символов</template>
          <template v-else-if="!$v.lastName.alpha">Фамилия должно содержать только буквы кириллицы</template>
          <template v-else>Фамилия обязательно для заполнения</template>
        </div>
      </div>

      <div class="field">
        <label for="patronomic">Отчество</label>
        <input id="patronomic" type="text" />
      </div>

      <div class="field">
        <label for="tel">Телефон</label>
        <input id="tel" type="text" v-model="tel" @blur="$v.tel.$touch()" />
        <div
          class="error"
          v-if="$v.tel.$error"
        >Телефонный номер должен быть в формате 8***-***-**-**</div>
      </div>

      <div class="field">
        <label for="birth_date">Дата рождения</label>
        <input type="date" id="birth_date" v-model="birthDate" @blur="$v.birthDate.$touch()" />
        <div class="error" v-if="$v.birthDate.$error">Поле обязательно для заполнения</div>
      </div>

      <div class="field">
        <label for="gender">Укажите пол</label>
        <select id="gender">
          <option value="male">Мужчина</option>
          <option value="famale">Женщина</option>
        </select>
      </div>

      <div class="field">
        <label for="client_group">Группа клиентов</label>
        <select multiple size="3" id="client_group" v-model="clientGroup" @blur="$v.clientGroup.$touch()">
          <option value="vip">VIP</option>
          <option value="trouble">Проблемные</option>
          <option value="oms">ОМС</option>
        </select>
        <div class="error" v-if="$v.clientGroup.$error">Поле обязательное для выбора</div>
      </div>

      <div class="field">
        <label for="doctor">Укажите лечащего врача</label>
        <select id="doctor">
          <option value="1">Иванов</option>
          <option value="2">Захаров</option>
          <option value="3">Чернышева</option>
        </select>
      </div>

      <div class="field">
        <label for="push_sms">Не отпровлять СМС</label>
        <input type="checkbox" id="push_sms" />
      </div>

      <div class="field">
        <label for="index">Индекс</label>
        <input type="text" id="index" />
      </div>

      <div class="field">
        <label for="country">Страна</label>
        <input type="text" id="country" />
      </div>

      <div class="field">
        <label for="region">Область</label>
        <input type="text" id="region" />
      </div>

      <div class="field">
        <label for="city">Город</label>
        <input type="text" id="city" v-model="city" @blur="$v.city.$touch()" />
        <div class="error" v-if="$v.city.$error">Поле обязательно для заполнения</div>
      </div>

      <div class="field">
        <label for="street">Улица</label>
        <input type="text" id="street" />
      </div>

      <div class="field">
        <label for="house">Дом</label>
        <input type="text" id="house" />
      </div>

      <div class="field">
        <label for="document">Выберите тип документа</label>
        <select id="document" v-model="documentType" @blur="$v.documentType.$touch()">
          <option value="1">Паспорт</option>
          <option value="2">Свидеьельство о рождении</option>
          <option value="3">Вод. удостоверение</option>
        </select>
        <div class="error" v-if="$v.documentType.$error">Поле обязательно для заполнения</div>
      </div>

      <div class="field">
        <label for="passport_data">Серия/номер документа</label>
        <input
          id="passport_data"
          type="text"
          v-model="passportData"
          @blur="$v.passportData.$touch()"
        />
        <div
          class="error"
          v-if="$v.passportData.$error"
        >Должны быть в формате 1234 567890</div>
      </div>

      <div class="field">
        <label for="issued_by">Кем выдан</label>
        <input id="issued_by" type="text" />
      </div>

      <div class="field">
        <label for="passport_date">Дата выдачи документа</label>
        <input
          id="passport_date"
          type="text"
          v-model="passportDate"
          @blur="$v.passportDate.$touch()"
        />
        <div
          class="error"
          v-if="$v.passportDate.$error"
        >Дата выдачи документа должна быть в формате ДД.ММ.ГГГГ</div>
      </div>

      <button id="button" type="submit" :disabled="$v.$invalid">Отправить форму</button>
    </form>
  </div>
</template>

<script>
import { required, minLength, maxLength } from "vuelidate/lib/validators";
import moment from "moment";

export default {
  data() {
    return {
      passportData: null,
      name: null,
      lastName: null,
      passportDate: null,
      tel: null,
      clientGroup: [],
      city: null,
      documentType: null,
      birthDate: null,
    };
  },

  validations: {
    passportData: {
      required,
      validFormat: (val) => /^\d{4} \d{6}$/.test(val),
    },
    passportDate: {
      required,
      validDate: (val) => moment(val, "DD.MM.YYYY", true).isValid(),
    },
    name: {
      required,
      maxLength: maxLength(10),
      alpha: (val) => /^[а-яё]*$/i.test(val),
    },
    lastName: {
      required,
      minLength: minLength(4),
      maxLength: maxLength(10),
      alpha: (val) => /^[а-яё]*$/i.test(val),
    },
    tel: {
      required,
      validFormat: (val) =>
        /^(7)?\(?\d{3}\)?[\s.-]?\d{3}[\s.-]?\d{2}[\s.-]?\d{2}$/.test(val),
    },
    clientGroup: {
      required,
    },
    city: {
      required,
    },
    documentType: {
      required,
    },
    birthDate: {
      required,
    },
  },

  methods: {
    someAction() {
      alert("Форма отправлена");
    },
  },
};
</script>

<style>



#container {
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  margin: auto;
  min-width: 200px;
  max-width: 1000px;
  font-size: 40px;
  border: 2px solid lime;
  border-radius: 0.5%;
  padding: 5px;
}

.field {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 24px;
  height: 70px;
}

.field > label {
  margin-right: 8px;
}

.field > input {
  height: 30px;
  font-size: 25px;
}

.field > .error {
  font-size: 20px;
  width: 100%;
  color: red;
}

::-webkit-calendar-picker-indicator {
  color: transparent;
  opacity: 0.7;
  background: url(https://t4.ftcdn.net/jpg/03/29/40/73/240_F_329407302_TwGc92ZKM9vDxJUgBVHT4d7F5UDuWZhp.jpg)
    no-repeat center;
  background-size: contain;
}

select {
  height: 30px;
  font-size: 20px;
}

#client_group {
  height: 77px;
}

#push_sms {
  margin-right: 8%;
  width: 20px;
}

#button {
  display: block;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  margin: auto;
  height: 40px;
  background-color: rgba(0, 162, 255, 0.932);
  border: 1px solid black;
  border-radius: 4px;
  font-size: 15px;
  font-weight: 800;
}

#button:focus {
  background-color: blue;
}

@media (max-width: 1024px) {
  #container {
    font-size: 40px;
  }
  .field {
    justify-content: space-around;
    margin-bottom: 140px;
  }

.field > input {
  height: 40px;
  font-size: 40px;
  width: 250px;
}

.field > .error {
  font-size: 22px;
  width: 100%;
  color: red;
}

#birth_date {
  font-size: 30px;
}

#button {
  margin-top: 160px;
}

select {
  height: 50px;
  font-size: 30px;
}

#client_group {
  height: 114px;
}
}

@media (max-width: 280px) {
  #container {
    font-size: 40px;
  }
  .field {
    margin-bottom: 140px;
  }

.field > input {
  height: 40px;
  font-size: 40px;
  width: 220px;
}

.field > .error {
  font-size: 22px;
  width: 100%;
  color: red;
}

#birth_date {
  font-size: 30px;
}

#button {
  margin-top: 260px;
}

select {
  height: 50px;
  font-size: 40px;
}
}
</style>