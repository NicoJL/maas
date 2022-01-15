<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated height-hint="90">
      <div class="q-pa-md">
        <q-toolbar class="bg-primary text-white">
          <q-toolbar-title>Maas V1.0 {{showMenu}}</q-toolbar-title>

          <div v-if="showMenu">
            <q-btn-dropdown
              auto-close
              stretch
              :label="user"
              class="flat round"
              icon="person_pin"
              color="primary"
            >
              <q-list>
                <q-item clickable v-on:click="closeSession">
                  <q-item-section>Cerrar Sesion</q-item-section>
                </q-item>
              </q-list>
            </q-btn-dropdown>
          </div>
        </q-toolbar>
      </div>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: 'MainLayout',
  data () {
    return {
      user: '',
      showMenu: false
    }
  },
  beforeRouteUpdate (to, from, next) {
    this.getToken()
    next()
  },
  created () {
    this.getToken()
  },
  methods: {
    closeSession () {
      localStorage.removeItem('token')
      this.showMenu = false
      this.$router.push({ path: '/' })
    },
    getToken () {
      const token = localStorage.getItem('token')
      if (token) {
        this.$axios.defaults.headers.common.Authorization = token
        this.showMenu = true
      }
    }
  }
}
</script>
