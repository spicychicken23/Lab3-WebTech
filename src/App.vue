<template>
  <div id="app" class="container mt-5">
    <div class="row">
      <div class="col-md-6">
        <div class="card">
          <div class="card-body">
            <form @submit.prevent="submitUser">
              <div class="mb-3">
                <label for="name" class="form-label">Name:</label>
                <input v-model="user.name" type="text" class="form-control" id="name" placeholder="Ahmad Hafiz" required>
              </div>
              <div class="mb-3">
                <label for="email" class="form-label">Email:</label>
                <input v-model="user.email" type="email" class="form-control" id="email" placeholder="coldest@gmail.com" required>
              </div>
              <button type="submit" class="btn btn-success me-2">Save</button>
              <button type="button" class="btn btn-primary" @click="updateUserfunction">Update</button>
            </form>
          </div>
        </div>
      </div>
      <div class="col-md-6">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">List of Users</h5>
            <hr>
            <div v-if="users.length === 0" class="text-muted">
              - No user created yet -
            </div>
            <div v-else>
              <div v-for="user in users" :key="user.id" class="mb-2 p-2 border rounded bg-light">
                <div>{{ user.name }}</div>
                <div class="text-muted">{{ user.email }}</div>
                <div class="mt-2">
                  <button @click="chooseUser(user)" class="btn btn-primary btn-sm me-2">Choose</button>
                  <button @click="removeUser(user.id)" class="btn btn-danger btn-sm">Remove</button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'

export default {
  name: 'App',
  data() {
    return {
      user: {
        name: '',
        email: ''
      },
      selectedUser: null
    }
  },
  computed: {
    ...mapState(['users'])
  },
  methods: {
    ...mapActions(['fetchUsers', 'createUser', 'updateUser', 'deleteUser']),
    submitUser() {
      if (this.user.name && this.user.email) {
        this.user.name = this.user.name.toUpperCase();
        this.createUser(this.user)
        this.user = { name: '', email: '' }
      }
    },
    chooseUser(user) {
      this.selectedUser = user
      this.user = { ...user }
    },
    updateUserfunction() {
      if (this.selectedUser && this.user.name && this.user.email) {
        this.updateUser({ id: this.selectedUser.id, ...this.user })
        this.selectedUser = null
        this.user = { name: '', email: '' }
      }
    },
    removeUser(userId) {
      if(window.confirm('Are you sure you want to remove this user?')){
        this.deleteUser(userId)
      }
    }
  },
  mounted() {
    this.fetchUsers()
  }
}
</script>

<style>

</style>