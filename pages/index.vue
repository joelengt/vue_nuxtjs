<template>
  <div>
    <h1>Sample  {{ sample }} </h1>

    <ul>
      <li v-for="item in items" :key="item.id">
        <p>{{ item.name }} - {{ item.price }}</p>
      </li>
    </ul>

    <button @click="getData">click</button>

    <div>
      <div class="container">
        <h2>Users</h2>
        <ul class="users">
          <li v-for="user in users" :key="user.id">
            <nuxt-link :to="'/users/'+user.id">{{ user.name }}</nuxt-link>
          </li>
        </ul>
      </div>
    </div>
    <div>
      <Logo/>
    </div>
    <Pet></Pet>
    <logito></logito>
  </div>

</template>

<script>

import axios from 'axios'
import Logo from '~/components/Logo.vue'
import Pet from '~/components/Pet.vue'

export default {
  components: {
    Logo,
    Pet
  },
  async asyncData({ req }) {
    const { data } = await axios.get('https://my-project-fzpynewkef.now.sh/')
    const items = data.items

    const response = await axios.get('https://jsonplaceholder.typicode.com/users')
    const users = response.data

    return {
      sample: 'dog',
      items,
      users
    }
  },
  data() {
    return {
      sample: '',
      formUsername: '',
      formPassword: '',
      items: []
    }
  },
  created() {

  },
  mounted() {

  },
  updated() {

  },
  methods: {
    async login() {
      try {
        await this.$store.dispatch('login', {
          username: this.formUsername,
          password: this.formPassword
        })
        this.formUsername = ''
        this.formPassword = ''
        this.formError = null
      } catch (e) {
        this.formError = e.message
      }
    },
    async logout() {
      try {
        await this.$store.dispatch('logout')
      } catch (e) {
        this.formError = e.message
      }
    },
    getData() {
      console.log('Get data')
      this.sample = 'changed!!'
    }
  }
}
</script>

<style>
.container {
  padding: 100px;
}
.error {
  color: red;
}
</style>
