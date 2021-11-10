<template>
  <div class="container">
    <div class="form">
      <form autocomplete="off" class="form" @submit.prevent="formSubmit">
        <h2>Личные данные</h2>
        <div class="row">
          <div class="col">
            <label for="surname" class="form-label"> Фамилия </label>
            <input
              v-model="formData.surname"
              id="surname"
              class="form-control"
            />
          </div>
          <div class="col">
            <label for="name" class="form-label"> Имя </label>
            <input v-model="formData.name" id="name" class="form-control" />
          </div>
          <div class="col">
            <label for="secondName" class="form-label"> Отчество </label>
            <input
              v-model="formData.secondName"
              id="secondName"
              class="form-control"
            />
          </div>
        </div>
        <div class="row">
          <div class="col-sm-3">
            <label for="birthDate" class="form-label"> Дата рождения </label>
            <input
              v-model="formData.birthDate"
              id="birthDate"
              placeholder="ДД.ММ.ГГГГ"
              class="form-control"
            />
          </div>
        </div>
        <div class="row">
          <div class="col-sm-12">
            <label for="email" class="form-label"> E-mail </label>
            <input
              v-model="formData.email"
              id="email"
              placeholder="test-email@mail.com"
              class="form-control"
            />
          </div>
        </div>
        <div class="row">
          <div class="col-sm-12">
            Пол
            <div class="form-check">
              <input
                v-model="formData.gender"
                id="male"
                type="radio"
                name="gender"
                value="Мужской"
                class="form-check-input"
                checked
              />
              <label for="male" class="form-check-label"> Мужской </label>
              <div>
                <input
                  v-model="formData.gender"
                  id="female"
                  type="radio"
                  name="gender"
                  value="Женский"
                  class="form-check-input"
                />
                <label for="female" class="form-check-label"> Женский </label>
              </div>
            </div>
          </div>
        </div>

        <h2>Паспортные данные</h2>
        <div class="row">
          <div class="col-sm-12">
            <!--            <select class="form-select" aria-label="Default select example">-->
            <!--              <option selected>Гражданство тест</option>-->
            <!--              <option value="1">One</option>-->
            <!--              <option value="2">Two</option>-->
            <!--              <option value="3">Three</option>-->
            <!--            </select>-->

            <div class="form-select" v-click-outside="hideDropdown">
              <label for="citizenship" class="form-label"> Гражданство </label>
              <input
                id="citizenship"
                @focus="isDropdownOpen = true"
                v-model="formData.citizenship"
                class="form-control"
              />
              <div v-if="isDropdownOpen">
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
          </div>

          <div>
            <div v-if="isPassportRussian">
              <div class="row">
                <div class="col-sm-3">
                  <label for="passportRusSerial"> Серия паспорта </label>
                  <input
                    id="passportRusSerial"
                    v-model="formData.passportRusSerial"
                    class="form-control"
                  />
                </div>
                <div class="col-sm-3">
                  <label for="passportRusNumber"> Номер паспорта </label>
                  <input
                    id="passportRusNumber"
                    v-model="formData.passportRusNumber"
                    class="form-control"
                  />
                </div>
                <div class="col-sm-6">
                  <label for="passportRusIssueDate"> Дата выдачи</label>
                  <input
                    id="passportRusIssueDate"
                    placeholder="ДД.ММ.ГГГГ"
                    v-model="formData.passportRusIssueDate"
                    class="form-control"
                  />
                </div>
              </div>
            </div>
            <div v-else>
              <div class="row">
                <div class="col-sm-6">
                  <label for="latinSurname"> Фамилия на латинице</label>
                  <input
                    v-model="formData.latinSurname"
                    id="latinSurname"
                    class="form-control"
                  />
                </div>
                <div class="col-sm-6">
                  <label for="latinName"> Имя на латинице </label>
                  <input
                    v-model="formData.latinName"
                    id="latinName"
                    class="form-control"
                  />
                </div>
              </div>
              <div class="form-text">
                Иностранцы заполняют латинскими буквами. Например Ivanov Ivan
              </div>
              <div class="row">
                <div class="col-sm-4">
                  <label for="passportForeignNumber"> Номер паспорта </label>
                  <input
                    id="passportForeignNumber"
                    v-model="formData.passportForeignNumber"
                    class="form-control"
                  />
                </div>
                <div class="col-sm-4">
                  <label for="passportForeignIssueCountry">
                    Страна выдачи
                  </label>
                  <input
                    id="passportForeignIssueCountry"
                    v-model="formData.passportForeignIssueCountry"
                    class="form-control"
                  />
                </div>
                <div class="col-sm-4">
                  <label for="passportForeignType"> Тип паспорта </label>
                  <input
                    id="passportForeignType"
                    list="passportForeignTypeList"
                    name="passportForeignType"
                    v-model="formData.passportForeignType"
                    class="form-select"
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
              </div>
            </div>

            <div class="row radio" @change="onNameChangedClicked()">
              <div class="row">
                <div class="col-sm-12">
                  Меняли ли фамилию или имя
                  <div class="form-check">
                    <input
                      v-model="formData.nameChanged"
                      id="no"
                      type="radio"
                      name="nameChanged"
                      value="Нет"
                      class="form-check-input"
                      checked
                    />
                    <label for="no"> Нет </label>
                    <div>
                      <input
                        v-model="formData.nameChanged"
                        id="yes"
                        type="radio"
                        name="nameChanged"
                        value="Да"
                        class="form-check-input"
                      />
                      <label for="yes"> Да </label>
                    </div>
                  </div>
                </div>
              </div>
            </div>

            <div v-if="isNameChanged" class="row">
              <div class="col-sm-6">
                <label for="previousSurname"> Предыдущая фамилия </label>
                <input
                  v-model="formData.previousSurname"
                  id="previousSurname"
                  class="form-control"
                />
              </div>
              <div class="col-sm-6">
                <label for="previousName"> Предыдущее имя </label>
                <input
                  v-model="formData.previousName"
                  id="previousName"
                  class="form-control"
                />
              </div>
            </div>
          </div>
          <div class="row-cols-sm-5">
            <button class="btn btn-success">Отправить</button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
// const bootstrap = require('bootstrap')
import ClickOutside from "vue-click-outside";
import citizenship from "../../src/assets/data/citizenships.json";
import passportTypes from "../../src/assets/data/passport-types.json";

// console.log(citizenship);
// console.log(passportTypes);
// console.log(bootstrap);

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
        passportRusSerial: "",
        passportRusNumber: "",
        passportForeignNumber: "",
        passportRusIssueDate: "",
        passportForeignIssueCountry: "",
        passportForeignType: "",
        passportForeignTypeList: "",
        previousSurname: "",
        previousName: "",
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
    formSubmit() {
      console.log("UPDATE API EVENT", this.formData);
    },
  },
  directives: {
    ClickOutside,
  },
};
</script>

<style scoped></style>
