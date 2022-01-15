<template>
  <q-page padding>
    <div class="row">
      <div class="col-md-4 offset-md-4">
        <q-card class="bg-primary my-card">
          <q-card-section>
            <div class="row">
              <div class="col-sm-12 text-center">
                <q-avatar
                  size="100px"
                  color="primary"
                  text-color="white"
                  icon="person"
                />
              </div>
            </div>
          </q-card-section>

          <q-card-section>
            <div class="row q-col-gutter-md">
              <div class="col-sm-12">
                <q-input v-model="email" type="email" bg-color="white" prefix="EMAIL:" ref="email" :rules="[val => val !== null && val !== ''|| 'Campo requerido']">
                  <template v-slot:prepend>
                    <q-icon name="mail" />
                  </template>
                </q-input>
              </div>
            </div>
            <div class="row q-col-gutter-md q-pt-md">
              <div class="col-sm-12">
                <q-input v-model="password" prefix="CONTRASEÑA:" bg-color="white" ref="password" :type="isPwd ? 'password' : 'text'" :rules="[val => val !== null && val !== ''|| 'Campo requerido']">
                  <template v-slot:prepend>
                    <q-icon name="lock" />
                  </template>
                   <template v-slot:append>
                  <q-icon
                    :name="isPwd ? 'visibility_off' : 'visibility'"
                    class="cursor-pointer"
                    @click="isPwd = !isPwd"
                  />
                  </template>
                </q-input>
              </div>
            </div>
          </q-card-section>

          <q-card-actions align="center">
            <q-btn  color="white" text-color="black" label="INICIAR SESIÓN" v-on:click="acceso" />
          </q-card-actions>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script>
import { api } from 'boot/axios'

export default {
  // name: 'PageName',
  data () {
    return {
      email: '',
      password: '',
      isPwd: true
    }
  },
  methods: {
    acceso () {
      this.$refs.email.validate()
      this.$refs.password.validate()
      if (this.$refs.email.hasError || this.$refs.password.hasError) {
        this.$q.notify({
          type: 'negative',
          message: 'Los valores ingresados son incorrectos'
        })
      } else {
        api
          .post('login', {
            email: this.email.toUpperCase(),
            password: this.password
          })
          .then(res => {
            console.log(res)
            if (res.data.status) {
              localStorage.setItem('token',res.data.token)
              this.$router.push({ path: '/horarios' })
            } else {
              this.$q.notify({
                type: 'negative',
                message: 'Los valores ingresados son incorrectos'
              })
            }
          })
      }
    }
  }
}
</script>
