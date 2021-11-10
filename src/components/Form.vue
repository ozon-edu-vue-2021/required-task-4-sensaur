<template>
  <div class="form">
    <div class="row">
      {{ formData }}
    </div>
    <form autocomplete="off" class="form">
      <h2>Личные данные</h2>

      <div class="row">
        <label for="surname"> Фамилия </label>
        <input v-model="formData.surname" id="surname" />
        <label for="name"> Имя </label>
        <input v-model="formData.name" id="name" />
        <label for="secondName"> Отчество </label>
        <input v-model="formData.secondName" id="secondName" />
      </div>

      <div class="row">
        <label for="birthDate"> Дата рождения </label>
        <input
          v-model="formData.birthDate"
          id="birthDate"
          placeholder="ДД.ММ.ГГГГ"
        />
      </div>

      <div class="row">
        <label for="email"> E-mail </label>
        <input
          v-model="formData.email"
          id="email"
          placeholder="test-email@mail.com"
        />
      </div>

      <h2>Пол</h2>
      <div class="row radio">
        <input
          v-model="formData.gender"
          id="male"
          type="radio"
          name="gender"
          value="Мужской"
          checked
        />
        <label for="male"> Мужской </label>
        <input
          v-model="formData.gender"
          id="female"
          type="radio"
          name="gender"
          value="Женский"
        />
        <label for="female"> Женский </label>
      </div>

      <h2>Паспортные данные</h2>
      <div class="row">
        <div class="citizenship-selector" v-click-outside="hideDropdown">
          <label for="citizenship"> Гражданство </label>
          <input
            id="citizenship"
            @focus="isDropdownOpen = true"
            v-model="formData.citizenship"
          />
          <div v-if="isDropdownOpen" class="citizenship-selector___dropdown">
            <ul v-if="allCitizenship.length">
              <li
                v-for="citizenship in allCitizenship"
                :key="citizenship._id"
                @click="onCitizenshipClicked(citizenship)"
              >
                {{ citizenship.nationality }}
              </li>
            </ul>
            <div v-else class="empty">Ничего на найдено</div>
          </div>
        </div>

        <div>
          <div v-if="isPassportRussian">
            <label for="passportRusSerial"> Серия паспорта </label>
            <input
              id="passportRusSerial"
              v-model="formData.passportRusSerial"
            />
            <label for="passportRusNumber"> Номер паспорта </label>
            <input
              id="passportRusNumber"
              v-model="formData.passportRusNumber"
            />
            <label for="passportRusIssueDate"> Дата выдачи</label>
            <input
              id="passportRusIssueDate"
              placeholder="ДД.ММ.ГГГГ"
              v-model="formData.passportRusIssueDate"
            />
          </div>
          <div v-else>
            <div class="row">
              <label for="latinSurname"> Фамилия на латинице</label>
              <input v-model="formData.latinSurname" id="latinSurname" />
              <label for="latinName"> Имя на латинице </label>
              <input v-model="formData.latinName" id="latinName" />
            </div>
            <div class="row">
              Иностранцы заполняют латинскими буквами. Например Ivanov Ivan
            </div>
            <label for="passportForeignNumber"> Номер паспорта </label>
            <input
              id="passportForeignNumber"
              v-model="formData.passportForeignNumber"
            />
            <label for="passportForeignIssueCountry"> Страна выдачи </label>
            <input
              id="passportForeignIssueCountry"
              v-model="formData.passportForeignIssueCountry"
            />

            <label for="passportForeignType"> Тип паспорта </label>
            <input
              id="passportForeignType"
              list="passportForeignTypeList"
              name="passportForeignType"
              v-model="formData.passportForeignType"
            />
            <datalist id="passportForeignTypeList">
              <option
                v-for="passportTypes in allPassportTypes"
                :key="passportTypes.id"
              >
                {{ passportTypes.type }}
              </option>
            </datalist>
          </div>
          <div class="row radio" @change="onNameChangedClicked()">
            <div>Меняли ли фамилию или имя</div>
            <input
              v-model="formData.nameChanged"
              id="yes"
              type="radio"
              name="nameChanged"
              value="Да"
              checked
            />
            <label for="yes"> Да </label>
            <input
              v-model="formData.nameChanged"
              id="no"
              type="radio"
              name="nameChanged"
              value="Нет"
            />
            <label for="no"> Нет </label>
          </div>
          <div v-if="isNameChanged">
            <label for="PreviousSurname"> Фамилия </label>
            <input v-model="formData.PreviousSurname" id="PreviousSurname" />
            <label for="PreviousName"> Имя </label>
            <input v-model="formData.PreviousName" id="PreviousName" />
          </div>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import ClickOutside from "vue-click-outside";
import citizenship from "../../src/assets/data/citizenships.json";
import passportTypes from "../../src/assets/data/passport-types.json";

// console.log(citizenship);
console.log(passportTypes);

export default {
  data() {
    return {
      formData: {
        surname: "",
        name: "",
        latinSurname: "",
        latinName: "",
        secondName: "",
        birthDate: "",
        email: "",
        gender: "",
        citizenship: "",
        passportTypes: "",
        passportRusSerial: "",
        passportRusNumber: "",
        passportForeignNumber: "",
        passportRusIssueDate: "",
        passportForeignIssueCountry: "",
        passportForeignType: "",
        passportForeignTypeList: "",
        PreviousSurname: "",
        PreviousName: "",
      },
      isDropdownOpen: false,
      isPassportRussian: false,
      isNameChanged: false,
      allCitizenship: citizenship,
      allPassportTypes: passportTypes,
    };
  },
  methods: {
    hideDropdown() {
      this.isDropdownOpen = false;
    },
    onCitizenshipClicked(selectedCitizenship) {
      // selectedCitizenship.nationality = undefined;
      this.formData.citizenship = selectedCitizenship.nationality;
      this.isPassportRussian = this.formData.citizenship === "Russia";
      this.hideDropdown();
    },
    onNameChangedClicked() {
      this.formData.nameChanged === "Да"
        ? (this.isNameChanged = true)
        : (this.isNameChanged = false);
    },
  },
  directives: {
    ClickOutside,
  },
};
</script>

<style scoped>
.form {
  border-radius: 6px;
  border: 1px solid #ccd8e4;
  background: white;
  max-width: 1000px;
  margin: 0 auto;
  padding: 40px;
  overflow: auto;
}

.citizenship-selector___dropdown {
}
</style>
