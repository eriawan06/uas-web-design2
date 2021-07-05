<template>
  <v-app>
    <c-header />

    <c-side-bar />

    <v-main>
      <v-slide-x-transition mode="out-in">
        <router-view></router-view>
      </v-slide-x-transition>
    </v-main>

    <c-footer />

    <keep-alive>
      <v-dialog
        v-model="dialog"
        fullscreen
        hide-overlay
        transition="dialogbottom-transition"
      >
        <component :is="currentComponent"></component>
      </v-dialog>
    </keep-alive>

    <c-alert />
  </v-app>
</template>

<style type="text/css">
.v-toolbar {
  flex: 0 !important;
}
.v-application .py-3 {
  text-align: center !important;
}
.v-card__text {
  text-align: center !important;
}
</style>

<script>
import { mapActions, mapGetters } from "vuex"

import CHeader from "@/components/CHeader.vue"
import CFooter from "@/components/CFooter.vue"
import CSideBar from "@/components/CSideBar.vue"
import CAlert from "@/components/CAlert.vue"
import Search from "@/views/Search.vue"
import Login from '@/views/Login.vue'
import Register from '@/views/Register.vue'

export default {
  name: "App",
  components: {
    CHeader,
    CFooter,
    CSideBar,
    CAlert,
    Search,
    Login,
    Register,
  },
  methods: {
    ...mapActions({
      setStatusDialog: "dialog/setStatus",
    }),
  },
  computed: {
    ...mapGetters({
      statusDialog: "dialog/status",
      currentComponent: "dialog/component",
    }),
    dialog: {
      get() {
        return this.statusDialog
      },
      set(value) {
        this.setStatusDialog(value)
      },
    },
  },
}
</script>
