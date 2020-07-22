<template>
  <v-app id="inspire">
    <v-content>
      <v-container
        class="fill-height"
        fluid
      >
        <v-row
          align="center"
          justify="center"
        >
          <v-col
            cols="12"
            sm="8"
            md="4"
          >
            <!--            <v-img :src="`/images/Nikky.png`" />-->
            <v-card class="elevation-12" v-if="!switchLoginRegister">
              <v-toolbar
                color="primary"
                dark
                flat
              >
                <v-toolbar-title>Login form</v-toolbar-title>
              </v-toolbar>
              <v-card-text>
                <v-form id="form" ref="form" v-model="valid" lazy-validation >
                  <v-text-field
                    v-model="login.username"
                    label="E-mail"
                    name="login"
                    prepend-icon="person"
                    type="text"
                    required
                  />
                  <v-text-field
                    id="password"
                    :rules="passwordRules"
                    v-model="login.password"
                    label="Password"
                    name="password"
                    prepend-icon="lock"
                    type="password"
                    required
                  />
                </v-form>
              </v-card-text>
              <v-card-actions>
                <v-btn elevation="0" @click="switcherLoginRegister"><span style="color: blue">Sign up</span></v-btn>
                <v-spacer />
                <v-btn :disabled="!valid" color="primary" @click="userLog">Login</v-btn>
              </v-card-actions>
            </v-card>
            <v-card class="elevation-12" v-if="switchLoginRegister">
              <v-toolbar
                color="primary"
                dark
                flat
              >
                <v-toolbar-title>Register form</v-toolbar-title>
              </v-toolbar>
              <v-card-text>
                <v-form id="form2" ref="form2" v-model="valid" lazy-validation >
                  <v-text-field
                    v-model="register.username"
                    label="Username"
                    name="login"
                    prepend-icon="person"
                    type="text"
                    required
                  />
                  <v-text-field
                    v-model="register.firstname"
                    label="Firstname"
                    name="login"
                    prepend-icon="person"
                    type="text"
                    required
                  />
                  <v-text-field
                    v-model="register.lastname"
                    label="Lastname"
                    name="login"
                    prepend-icon="person"
                    type="text"
                    required
                  />
                  <v-text-field
                    v-model="register.email"
                    label="E-mail"
                    :rules="emailRules"
                    name="login"
                    prepend-icon="person"
                    type="text"
                    required
                  />
                  <v-text-field
                    id="password"
                    :rules="passwordRules"
                    v-model="register.password"
                    label="Password"
                    name="password"
                    prepend-icon="lock"
                    type="password"
                    required
                  /><v-text-field
                  id="password"
                  :rules="passwordRules"
                  v-model="register.password_confirmation"
                  label="Repeat Password"
                  name="password"
                  prepend-icon="lock"
                  type="password"
                  required
                />
                </v-form>
              </v-card-text>
              <v-card-actions>
                <v-btn elevation="0" @click="switcherLoginRegister"><span style="color: blue">Sign in</span></v-btn>
                <v-spacer />
                <v-btn :disabled="!valid" color="primary" @click="registerUser">Login</v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
    export default {
        props: {
            source: String,
        },
        data(){
            return{
                switchLoginRegister: false,
                login: {
                    username: '',
                    password: '',
                },
                register: {
                    terms_accepted: true,
                    email: '',
                    firstname: '',
                    lastname: '',
                    username: '',
                    password: '',
                    password_confirmation: ''
                },
                valid: true,
                passwordRules: [
                    v => !!v || 'Password is required',
                    v => (v && v.length >= 6) || 'Password must minimum 6 characters',
                ],
                emailRules: [
                    v => !!v || 'E-mail is required',
                    v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
                ],
            }
        },
        methods: {
            switcherLoginRegister(){
              this.switchLoginRegister = !this.switchLoginRegister
            },
          async userLog(){
                try{
                    if(!this.$refs.form.validate()) return
                    const res = await this.$auth.loginWith('local', {
                      data: this.login
                    })
                } catch (e) {
                    this.$toast.error('Error')
                }


            },
            async registerUser(){
                try{
                    if(!this.$refs.form2.validate()) return
                    const res = await this.$axios.post('auth/register', this.register)
                    console.log(res)
                } catch(e) {
                    this.$toast.error('Error')
                }
            }
        }
    }
</script>



<style>

</style>
