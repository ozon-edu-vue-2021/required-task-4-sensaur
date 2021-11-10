<template>
  <div class="form">
    <form
      autocomplete="off"
      class="form"
    >
      <h2> Личные данные </h2>
      <div class="row">
        {{ formData }}
      </div>
      <div class="row">
        <label for="surname"> Фамилия </label>
        <input
          id="surname"
        />
        <label for="name"> Имя </label>
        <input
          id="name"
        />
        <label for="secondName"> Отчество </label>
        <input
          id="secondName"
        />
      </div>

      <div class="row">
        <label for="birthDate"> Дата рождения </label>
        <input
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

      <h2> Пол </h2>

      <div class="row radio">
        <input
          type="radio"
        />
        <span for="gender"> Мужской </span>
        <input
          type="radio"
        />
        <span for="gender"> Женский </span>
      </div>


      <h2> Паспортные данные </h2>
      <div class="row">
        <div class="skill-selector"
             v-click-outside="hideDropdown">
          <label for="skills"> Гражданство </label>
          <input
            id="skills"
            @focus="isDropdownOpen = true"
          />

          <div
            v-if="isDropdownOpen"
            class="skill-selector___dropdown"
          >
            <ul v-if="allSkills.length">
              <li
                v-for="skill in allSkills"
                :key="skill._id"
                @click="onSkillClicked(skill)"
              >
                {{ skill.nationality }}
              </li>
            </ul>
            <div
              v-else
              class="empty">
              Ничего на найдено
            </div>
          </div>

        </div>
      </div>
    </form>
  </div>
</template>

<script>
import ClickOutside from "vue-click-outside";
import skills from "../../src/assets/data/citizenships.json";

console.log(skills);

export default {
  data() {
    return {
      formData: {
        email: "",
        citizenship: "",
        skills: []
      },
      isDropdownOpen: false,
      allSkills: skills
    };
  },
  methods: {
    hideDropdown() {
      this.isDropdownOpen = false;
    },
    onSkillClicked(selectedSkill) {
      this.formData.skills.push(selectedSkill);
      this.hideDropdown()
    }
  },
  directives: {
    ClickOutside
  }
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
</style>
