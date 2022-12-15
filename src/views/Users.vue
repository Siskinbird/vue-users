<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <!--table-->
        <table>

          <!--table head-->
          <thead>
          <tr>
            <th @click="sort('name')">Name &#8595;	</th>
            <th @click="sort('age')">Age &#8595;	</th>
            <th @click="sort('gender')">Gender &#8595;	</th>
          </tr>
          </thead>

          <!--table body-->
          <tbody>
          <tr v-for="user in usersSort" :key="user.id">
            <td>
              <img :src="user.img" :alt="user.name">
              <span>{{user.name}}</span>
            </td>
            <td>{{user.age}}</td>
            <td>{{user.gender}}</td>
          </tr>
          </tbody>

        </table>
        <p>debug: sort: {{ currentSort }}, dir: {{ currentSortDir }} </p>
      </div>
    </section>
    <section>
      <div class="container">
        <div class="button-list">
          <button class="btn btnPrimary"> &#8610; </button>
          <button class="btn btnPrimary"> &#8611; </button>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      users: [],
      currentSort: 'name',
      currentSortDir: 'asc'
    }
  },
  created() {
    axios
        .get("https://tocode.ru/static/_secret/courses/1/usersCrmApi.php")
        .then(response => this.users = response.data)
        .catch(err => console.log(err))
  },
  computed: {
    usersSort () {
      return this.users.sort((a, b) => {
        let mod = 1
        if (this.currentSortDir === 'desc') {
          mod = -1
        }
        if (a[this.currentSort] < b[this.currentSort]) {
          return -1 * mod
        }
        if (a[this.currentSort] > b[this.currentSort]) {
          return mod
        }
        return 0

      })
    }
  },
  methods: {
    sort(e) {
      if (e === this.currentSort) {
        this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc'
      }
      this.currentSort = e
    }
  }
}
</script>
<style lang="scss" scoped>
img {
  width: 60px;
  height: auto;
  border-radius: 50%;
  margin-right: 16px;
}

</style>