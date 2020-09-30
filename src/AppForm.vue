<template>
  <div v-if="this.submitted === true">
    <AppFormSubmitted />
  </div>

  <div v-if="this.submitted === false">
    <form @submit.prevent="formSubmit">
      <div class="form" v-show="step === 1">
        <div class="form__field">
          <input
            required
            type="text"
            id="firstName"
            name="firstName"
            v-model="user.firstName"
            class="form__input"
          />
          <label for="firstName" class="form__label">
            <span class="form__label--name"> Jméno </span>
          </label>
        </div>

        <div class="form__field">
          <input
            required
            type="text"
            id="lastName"
            name="lastName"
            v-model="user.lastName"
            class="form__input"
          />
          <label for="lastName" class="form__label"
            ><span class="form__label--name"> Příjmení </span></label
          >
        </div>

        <div class="form__field">
          <input
            required
            id="email"
            name="email"
            type="email"
            v-model="user.email"
            class="form__input"
          />
          <label for="email" class="form__label"
            ><span class="form__label--name"> Email </span></label
          >
        </div>

        <div class="form__field">
          <input
            id="password"
            name="password"
            type="password"
            v-model="user.password"
            class="form__input"
          />
          <label for="password" class="form__label"
            ><span class="form__label--name"> Heslo </span></label
          >
        </div>

        <button class="form__button" @click.prevent="next()">&#8594;</button>
      </div>

      <div class="form" v-show="step === 2">
        <div class="form__field form__field--select">
          <select id="gender" name="gender" v-model="user.gender" required>
            <option value="male">muž</option>
            <option value="female">žena</option>
          </select>
          <label for="gender" class="form__label">Pohlaví:</label>
        </div>

        <div class="form__field form__field--select">
          <label for="birthday" class="form__label">Datum narození:</label>
          <input
            required
            type="date"
            id="birthday"
            name="birthday"
            min="1900-01-01"
            max="2020-01-01"
            v-model="user.birthday"
            class="form__input"
          />
        </div>

        <div class="form__field form__field--checkbox">
          <input
            type="checkbox"
            name="newsletter"
            id="newsletter"
            v-model="user.newsletter"
            class="form__input"
          />
          <label for="newsletter">Mám zájem o newsletter</label>
        </div>

        <div class="form__field form__field--checkbox">
          <input type="checkbox" required id="agreement" class="form__input" />
          <label for="agreement">Souhlasím s podmínkami</label>
        </div>

        <button class="form__button" @click.prevent="prev()">&#8592;</button>
        <button class="form__button form__button--primary" type="submit">
          Odeslat
        </button>
      </div>
    </form>
    <div class="error" v-if="errors.length">
      <ul>
        <li v-for="(error, i) in errors" :key="i">{{ error }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import AppFormSubmitted from './AppFormSubmitted';

export default {
  name: 'AppForm',
  components: { AppFormSubmitted },
  data() {
    return {
      step: 1,
      errors: [],
      submitted: false,
      user: {
        firstName: null,
        lastName: null,
        email: null,
        password: null,
        birthday: null,
        gender: null,
        newsletter: null,
      },
    };
  },
  computed: {
    passwordValidation() {
      let errors = [];
      for (let condition of this.passwordRules) {
        if (!condition.regex.test(this.password)) {
          errors.push(condition.message);
        }
      }
      if (errors.length === 0) {
        return { valid: true, errors };
      } else {
        return { valid: false, errors };
      }
    },
  },
  methods: {
    formSubmit() {
      fetch('https://jsonplaceholder.typicode.com/posts', {
        method: 'POST',
        body: JSON.stringify({
          firstName: this.user.firstName,
          lastName: this.user.lastName,
          email: this.user.email,
          password: this.user.password,
          birthday: this.user.birthday,
          gender: this.user.gender,
          marketingConsent: this.user.newsletter,
        }),
        headers: {
          'Content-type': 'application/json; charset=UTF-8',
        },
      })
        .then((response) => response.json())
        .then((json) => console.log(json));

      this.submitted = true;
    },
    prev() {
      this.step--;
    },
    next() {
      if (
        this.user.firstName &&
        this.user.lastName &&
        this.user.email &&
        this.user.password
      ) {
        this.step++;
      }

      this.errors = [];

      if (!this.user.firstName) {
        this.errors.push('Vyplňte Vaše jméno.');
      }
      if (!this.user.lastName) {
        this.errors.push('Vyplňte Vaše příjmení.');
      }
      if (!this.user.email) {
        this.errors.push('Zadejte prosím platný email.');
      }
      if (!this.user.password) {
        this.errors.push('Neplatné heslo.');
      }
    },
  },
};
</script>

<style lang="scss">
$primary-color: #0011fa7a;
$error-color: #df2e2e;
$text-color: #020202;
$text-color-light: #fefefe;

.form {
  margin: 2rem auto;
  width: 40%;
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-direction: column;
  background: #f8f8f8;
  padding: 1rem;
  border-radius: 8px;

  &__field {
    width: 90%;
    position: relative;
    height: 80px;

    &--select {
      display: flex;
      flex-direction: column;
      justify-content: flex-end;
      height: 100px;

      select {
        font-size: 1rem;
        padding: 0.5em 1rem;
        margin: 1rem 0;
        border: 1px solid rgba(170, 170, 170, 0.397);
        box-shadow: 0 1px 0 1px rgba(0, 0, 0, 0.04);
        border-radius: 0.5em;
        background-color: #f8f8f8;
      }
    }

    &--checkbox {
      height: 4rem;
      display: flex;
      align-items: center;
      font-weight: 300;

      input[type='checkbox'] {
        width: 20px;
        margin: 0 0.6rem 0 0;
      }
    }
  }

  &__label {
    position: absolute;
    left: 0;
    width: 100%;
    height: 100%;
    font-weight: 300;
    font-size: 1rem;
    text-transform: uppercase;
    letter-spacing: 2px;
    line-height: 2.5rem;
    pointer-events: none;
    border-bottom: 1px solid rgb(228, 228, 228);
    color: $text-color;
  }

  &__input {
    width: 100%;
    height: 100%;
    padding-top: 1.5rem;
    color: #595f6e;
    background-color: #f8f8f8;
    font-size: 1rem;
    border: none;
    outline: none;
    &:focus + .form__label .form__label--name {
      color: $primary-color;
    }
  }

  &__button {
    background-color: $primary-color;
    color: white;
    padding: 1rem 1.5rem;
    border: none;
    border-radius: 8px;
    font-size: 1.2rem;
    cursor: pointer;
    transition: background-color 250ms;
    margin: 2rem 0.5rem 0.5rem 0.5rem;
    width: 85%;

    &--primary {
      background-color: #242ec0;
    }
  }
}

.error {
  background: $error-color;
  color: $text-color-light;
  width: 40%;
  padding: 3rem;
  margin: 2rem auto;
}

//changes colors from autocopmlete defaults
input:-webkit-autofill {
  -webkit-box-shadow: 0 0 0 50px #f8f8f8 inset;
  -webkit-text-fill-color: rgba(0, 0, 0, 0.8);
}

input:-webkit-autofill:focus {
  -webkit-box-shadow: 0 0 0 50px #f8f8f8 inset;
  -webkit-text-fill-color: rgba(0, 0, 0, 0.8);
}
</style>
