<template>
  <v-row>
    <v-col cols="12">
      <v-card max-width="400" class="mx-auto" flat>
        <v-alert type="info" :value="!isError">
          이메일과 비밀번호를 입력 해주세요.
        </v-alert>
        <v-alert type="warning" :value="isError">
          이메일 혹은 비밀번호가 틀립니다.
        </v-alert>
      </v-card>
    </v-col>
    <v-col>
      <v-row justify="center">
        <h2>로그인</h2>
      </v-row>
    </v-col>
    <v-col cols="12">
      <v-card max-width="400" class="mx-auto text-end" flat>
        <v-form ref="form" v-model="valid">
          <v-text-field v-model="email" :rules="emailRules" label="E-mail" required></v-text-field>
          <v-text-field
            :append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
            v-model="password"
            :rules="passwordRules"
            :type="show ? 'text' : 'password'"
            label="Password"
            hint="At least 8 characters"
            required
            @click:append="show = !show"
          ></v-text-field>
          <br />
          <v-btn :disabled="!valid" color="teal white--text" @click="signIn">
            로그인
          </v-btn>
        </v-form>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import SIGNIN from '@/graphql/signin.gql'
import { mapState, mapMutations } from 'vuex'

export default {
  data() {
    return {
      valid: true,
      name: '',
      nameRules: [v => !!v || 'Name is required', v => (v && v.length <= 10) || 'Name must be less than 10 characters'],
      email: '',
      emailRules: [v => !!v || 'E-mail is required', v => /.+@.+\..+/.test(v) || 'E-mail must be valid'],
      password: '',
      show: false,
      passwordRules: [v => !!v || 'Password is required', v => v.length >= 8 || 'Password min 8 characters'],
      checkbox: false,
      alertvalue: null,
      account: null,
      PubKey: null,
      nexttwo: null,
      isError: false,
    }
  },
  computed: {
    ...mapState(['isLogin']),
  },
  methods: {
    ...mapMutations(['tokenSave']),
    onDone() {
      this.alertvalue = true
    },
    signIn() {
      this.$apollo
        .mutate({
          mutation: SIGNIN,
          variables: {
            email: this.email,
            password: this.password,
          },
        })
        .then(result => {
          console.log(result.data.signIn.token)
          this.$store.state.accessToken = result.data.signIn.token
          this.$store.state.isLogin = true
          this.tokenSave()
          this.$router.push({ name: 'Home' })
        })
        .catch(() => {
          console.log('에러입니다')
          // console.log(error)
          this.isError = true
        })
    },
  },
}
</script>

<style></style>
