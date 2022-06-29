<template>
  <v-app-bar
    light
    app
    class="align-baseline justify-sm-center align-content-center"
  >
    <v-btn icon color="#00ACA2" class="ml-5">
      <v-icon x-large>mdi-waves</v-icon>
    </v-btn>
    <v-toolbar-title v-text="title" />
    <v-spacer />
    <small-search-form
      v-if="search"
      class="align-content-center mt-6"
      short
    ></small-search-form>
    <v-spacer />
    <v-btn color="#00ACA2"  filled rounded solo outlined plain raised text class="mr-md-2"
      >Вход</v-btn
    >
    <v-btn color="#00ACA2"  outlined plain raised rounded text>Регистрация</v-btn>
  </v-app-bar>
</template>

<script>
import { mdiBiohazard } from '@mdi/js'
import {mapGetters} from "vuex";
import SmallSearchForm from '../../components/forms/search/SmallSearchForm'



export default {
  name: 'BaseHeader',
  components: { SmallSearchForm},
  props: {
    search: {
      type: Boolean,
      default: false,
    },
  },
  data: () => ({
    logo: mdiBiohazard,
    title: '',
  }),
  computed: {
    ...mapGetters({

    }),
  },
  methods: {
    toggleVisible(type, value) {

    }
  }
}
</script>

<style scoped></style>
