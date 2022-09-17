<template>
  <div class="img js-fullheight background">
    <section class="ftco-section">
      <div class="container">
        <div class="row justify-content-center">
          <div class="col-md-6 text-center mb-5">
            <h2 class="heading-section">Account</h2>
          </div>
        </div>
        <div class="row justify-content-center">
          <div class="col-md-6 col-lg-4">
            <div class="login-wrap p-0">
              <form>
                <div class="form-group">
                  <input
                    v-model="email"
                    type="email"
                    class="form-control"
                    placeholder="Email"
                    required
                  />
                  <label v-if="errors.mail" class="error">
                    {{ errors.mail }}</label
                  >
                </div>
                <div class="form-group">
                  <input
                    v-model="password"
                    :type="passwordType"
                    class="form-control"
                    placeholder="Password"
                    required
                  />
                  <span
                    class="fa fa-fw fa-eye field-icon toggle-password"
                    :class="showPassword ? 'fa-eye-slash' : 'fa-ey'"
                    style="cursor: pointer"
                    @click="handleShowPassword"
                  ></span>
                </div>
                <label v-if="errors.password" class="error">
                  {{ errors.password }}</label
                >

                <div class="form-group">
                  <input
                    v-model="rePassword"
                    :type="passwordType"
                    class="form-control"
                    placeholder="Re-password"
                    required
                  />
                  <span
                    class="fa fa-fw fa-eye field-icon toggle-password"
                    :class="showPassword ? 'fa-eye-slash' : 'fa-ey'"
                    style="cursor: pointer"
                    @click="handleShowPassword"
                  ></span>
                  <label v-if="errors.rePassword" class="error">
                    {{ errors.rePassword }}</label
                  >
                </div>

                <div class="form-group">
                  <label for="role">Role: </label>
                  <select
                    id="role"
                    v-model="userRole"
                    class="form-control select"
                  >
                    <option key="webDev" value="webDev">
                      I'm a Web Developer
                    </option>
                    <option key="webDes" value="webDes">
                      I'm a Web Designer
                    </option>
                  </select>
                </div>

                <div class="form-group">
                  <label for="skill">Skills: </label>
                  <input
                    id="skill"
                    v-model="tempSkill"
                    type="text"
                    class="form-control"
                    placeholder="Skill"
                    @keyup="handleInputSkill"
                  />
                  <div class="pill-container">
                    <label
                      v-for="(item, index) in skills"
                      :key="index"
                      class="pill"
                      :title="'Remove ' + item"
                    >
                      {{ item }}
                    </label>
                  </div>
                </div>
                <label v-if="errors.skills" class="error">
                  {{ errors.skills }}</label
                >

                <div class="form-group">
                  <label class="checkbox-wrap checkbox-primary"
                    >Accept terms and conditions
                    <input v-model="termAccepted" type="checkbox" />
                    <span class="checkmark"></span>
                  </label>
                  <label v-if="errors.term" class="error">
                    {{ errors.term }}</label
                  >
                </div>

                <div class="form-group">
                  <button
                    type="submit"
                    class="form-control btn btn-primary submit px-3"
                    :disabled="disableSubmitButton || isSending"
                    :title="
                      disableSubmitButton
                        ? 'You have to fill out the form to submit'
                        : isSending
                        ? 'Please wait...'
                        : 'Submit'
                    "
                    @click.prevent="handleSubmit"
                  >
                    Sign Up
                    <div v-if="isSending" class="my-loader"></div>
                  </button>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script lang="ts">
  import { defineComponent } from 'vue'
  export default defineComponent({
    data() {
      const skills: string[] = []

      return {
        email: '',
        password: '',
        rePassword: '',
        showPassword: false,
        passwordType: 'password',
        userRole: 'webDev',
        tempSkill: '',
        skills: skills,
        termAccepted: true,
        errors: {
          mail: '',
          password: '',
          rePassword: '',
          skills: '',
          term: '',
        },
        disableSubmitButton: true,
        isSending: false,
      }
    },
    updated() {
      console.log('Updated')
      this.validateInputs()
    },
    methods: {
      handleShowPassword() {
        this.showPassword = !this.showPassword
        this.passwordType = this.showPassword ? 'text' : 'password'
      },
      handleInputSkill(e: any) {
        const skill = this.tempSkill.slice(0, -1)
        if ([',', ' '].includes(e.key)) {
          if (this.tempSkill && !this.skills.includes(skill)) {
            this.skills.push(skill)
          }
          this.tempSkill = ''
        }
      },
      validateInputs() {
        let hasError = false
        if (!this.email || !this.email.includes('@')) {
          this.errors.mail = 'Your email is invalid!'
          hasError = true
        } else {
          this.errors.mail = ''
          hasError = false
        }

        if (this.password.length < 10) {
          this.errors.password = 'Your password too short!'
          hasError = true
        } else {
          this.errors.password = ''
          hasError = false
        }

        if (this.rePassword !== this.password) {
          this.errors.rePassword = 'The password and re-password are not same!'
          hasError = true
        } else {
          this.errors.rePassword = ''
          hasError = false
        }

        if (this.skills.length === 0) {
          this.errors.skills = 'You have to add at least one skill!'
          hasError = true
        } else {
          this.errors.skills = ''
          hasError = false
        }

        if (!this.termAccepted) {
          this.errors.term = 'You have to accept our term and conditions!'
          hasError = true
        } else {
          this.errors.term = ''
          hasError = false
        }

        this.disableSubmitButton = hasError && !this.isSending
      },
      handleSubmit() {
        this.isSending = true
        setTimeout(() => {
          alert(`
            Mail: ${this.email}
            Password: ${this.password}
            Role: ${this.userRole}
            Skill: ${this.skills}
            ==>Register successfully!`)
          this.isSending = false
        }, 3000)
      },
    },
  })
</script>

<style scoped>
  .background {
    background-image: url(./assets/images/bg.jpg);
    height: 100%;
  }
</style>
