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
      </div>
    </form>
  </div>
</template>

<script>
import ClickOutside from "vue-click-outside";
import citizenship from "../../src/assets/data/citizenships.json";

console.log(citizenship);

export default {
  data() {
    return {
      formData: {
        surname: "",
        name: "",
        secondName: "",
        birthDate: "",
        email: "",
        gender: "",
        citizenship: "",
      },
      isDropdownOpen: false,
      allCitizenship: citizenship,
    };
  },
  methods: {
    hideDropdown() {
      this.isDropdownOpen = false;
    },
    onCitizenshipClicked(selectedCitizenship) {
      this.formData.citizenship = selectedCitizenship.nationality;
      this.hideDropdown();
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
