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
              @input="onCyrillicInputSurname"
            />
            <span v-if="formData.cyrillicErrorsSurname" class="error">
              {{ formData.cyrillicErrorsSurname }}
            </span>
          </div>
          <div class="col">
            <label for="name" class="form-label"> Имя </label>
            <input
              v-model="formData.name"
              id="name"
              class="form-control"
              @input="onCyrillicInputName"
            />
            <span v-if="formData.cyrillicErrorsName" class="error">
              {{ formData.cyrillicErrorsName }}
            </span>
          </div>
          <div class="col">
            <label for="secondName" class="form-label"> Отчество </label>
            <input
              v-model="formData.secondName"
              id="secondName"
              class="form-control"
              @input="onCyrillicInputSecondName"
            />
            <span v-if="formData.cyrillicErrorsSecondName" class="error">
              {{ formData.cyrillicErrorsSecondName }}
            </span>
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
              @input="onEmailInput"
            />
            <span v-if="formData.emailErrors" class="error">
              {{ formData.emailErrors }}
            </span>
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
            <div class="form-select" v-click-outside="hideDropdown">
              <label for="citizenship" class="form-label">
                Гражданство {{ formData.citizenship }}</label
              >
              <!--              <div>-->
              <!--                <b-form-input list="my-list-id"></b-form-input>-->
              <!--                <datalist id="my-list-id">-->
              <!--                  <option-->
              <!--                      v-for="citizenship in allCitizenship"-->
              <!--                      :key="citizenship._id"-->
              <!--                  >-->
              <!--                    {{ citizenship.nationality }}-->
              <!--                  </option>-->
              <!--                </datalist>-->
              <!--              </div>-->
              <input
                id="citizenship"
                @focus="isDropdownOpen = true"
                v-model="searchWord"
                class="form-control"
              />
              <div v-if="isDropdownOpen">
                <ul v-if="filteredCitizenshipList.length">
                  <li
                    v-for="citizenship in filteredCitizenshipList"
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
                    list="passportForeignIssueCountryList"
                    v-model="formData.passportForeignIssueCountry"
                    class="form-select"
                  />
                  <datalist id="passportForeignIssueCountryList">
                    <option
                      v-for="citizenship in allCitizenship"
                      :key="citizenship._id"
                    >
                      {{ citizenship.nationality }}
                    </option>
                  </datalist>
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
                  @input="onCyrillicInputPreviousSurname"
                />
                <span
                  v-if="formData.cyrillicErrorsPreviousSurname"
                  class="error"
                >
                  {{ formData.cyrillicErrorsPreviousSurname }}
                </span>
              </div>
              <div class="col-sm-6">
                <label for="previousName"> Предыдущее имя </label>
                <input
                  v-model="formData.previousName"
                  id="previousName"
                  class="form-control"
                  @input="onCyrillicInputPreviousName"
                />
                <span v-if="formData.cyrillicErrorsPreviousName" class="error">
                  {{ formData.cyrillicErrorsPreviousName }}
                </span>
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
import ClickOutside from "vue-click-outside";
import citizenship from "../../src/assets/data/citizenships.json";
import passportTypes from "../../src/assets/data/passport-types.json";
import { throttle } from "@/helpers";

const EMAIL_REG_EXP = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
const CYRILLIC_REG_EXP = /^[?!,.а-яА-ЯёЁ]+$/;
// const SERIA_REG_EXP = /^d{4}$/;
// const NUMBER_REG_EXP = /^d{6}$/;
// const DATE_REG_EXP = /^[a-zA-Z]+$/;
// const LATIN_REG_EXP = /^[a-zA-Z]+$/;

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
        emailErrors: "",
        cyrillicErrorsName: "",
        cyrillicErrorsSurname: "",
        cyrillicErrorsSecondName: "",
        cyrillicErrorsPreviousSurname: "",
        cyrillicErrorsPreviousName: "",
      },
      isDropdownOpen: false,
      isPassportRussian: false,
      isNameChanged: false,
      allCitizenship: citizenship,
      allPassportTypes: passportTypes,
      throttledSearchCitizenship: null,
      searchWord: "",
    };
  },
  methods: {
    hideDropdown() {
      this.isDropdownOpen = false;
    },
    onCitizenshipClicked(selectedCitizenship) {
      this.formData.citizenship = selectedCitizenship.nationality;
      this.isPassportRussian = this.formData.citizenship === "Russia";
      this.searchWord = "";
      this.hideDropdown();
    },
    onNameChangedClicked() {
      this.formData.nameChanged === "Да"
        ? (this.isNameChanged = true)
        : (this.isNameChanged = false);
    },
    formSubmit() {
      if (
        !this.formData.emailErrors &
        !this.formData.cyrillicErrorsName &
        !this.formData.cyrillicErrorsSurname &
        !this.formData.cyrillicErrorsSecondName &
        !this.formData.cyrillicErrorsPreviousSurname &
        !this.formData.cyrillicErrorsPreviousName
      ) {
        console.log("UPDATE API EVENT", this.formData);
      } else {
        console.log("FAILED UPDATE API EVENT");
      }
    },
    getCitizenship() {
      console.log("searchWord==>", this.searchWord);
    },
    onEmailInput() {
      if (!EMAIL_REG_EXP.test(this.formData.email)) {
        this.formData.emailErrors = "Проверьте правильность ввода email";
      } else {
        this.formData.emailErrors = "";
      }
    },
    onCyrillicInputName() {
      if (!CYRILLIC_REG_EXP.test(this.formData.name)) {
        this.formData.cyrillicErrorsName = "заполняeтся кириллицей";
      } else {
        this.formData.cyrillicErrorsName = "";
      }
    },
    onCyrillicInputSecondName() {
      if (!CYRILLIC_REG_EXP.test(this.formData.secondName)) {
        this.formData.cyrillicErrorsSecondName = "заполняeтся кириллицей";
      } else {
        this.formData.cyrillicErrorsSecondName = "";
      }
    },
    onCyrillicInputSurname() {
      if (!CYRILLIC_REG_EXP.test(this.formData.surname)) {
        this.formData.cyrillicErrorsSurname = "заполняeтся кириллицей";
      } else {
        this.formData.cyrillicErrorsSurname = "";
      }
    },
    onCyrillicInputPreviousSurname() {
      if (!CYRILLIC_REG_EXP.test(this.formData.previousSurname)) {
        this.formData.cyrillicErrorsPreviousSurname = "заполняeтся кириллицей";
      } else {
        this.formData.cyrillicErrorsPreviousSurname = "";
      }
    },
    onCyrillicInputPreviousName() {
      if (!CYRILLIC_REG_EXP.test(this.formData.previousName)) {
        this.formData.cyrillicErrorsPreviousName = "заполняeтся кириллицей";
      } else {
        this.formData.cyrillicErrorsPreviousName = "";
      }
    },
  },
  directives: {
    ClickOutside,
  },
  created() {
    this.allCitizenship = citizenship;
    this.throttledSearchCitizenship = throttle(this.getCitizenship, 2000);
  },
  watch: {
    searchWord(newValue) {
      this.throttledSearchCitizenship(newValue);
    },
  },
  computed: {
    filteredCitizenshipList() {
      return this.allCitizenship.filter(
        (cur) =>
          cur.nationality
            .toLowerCase()
            .indexOf(this.searchWord.toLowerCase()) !== -1
      );
    },
  },
};
</script>

<style scoped>
.error {
  color: darkred;
}
</style>
