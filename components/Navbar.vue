<template>
  <div>
    <b-navbar style="background-color: #00C58E !important;" toggleable="lg" type="dark" variant="info">
      <b-navbar-brand href="/">Adaptech</b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <!-- <b-nav-form>
            <b-form-input size="sm" class="mr-sm-2" placeholder="Search"></b-form-input>
            <b-button size="sm" class="my-2 my-sm-0" type="submit">Search</b-button>
          </b-nav-form> -->

          <b-nav-item-dropdown text="Tickets" right>
            <b-dropdown-item href="/tickets/create">Create</b-dropdown-item>
            <b-dropdown-item href="/tickets">List</b-dropdown-item>
          </b-nav-item-dropdown>

          <b-nav-item-dropdown right>
            <!-- Using 'button-content' slot -->
            <template v-slot:button-content>
              <em>User</em>
            </template>
            <div v-if="access_token != '' && typeof access_token != 'undefined'">
              <b-dropdown-item href="/profile">Profile</b-dropdown-item>
              <b-dropdown-item href="#">Sign Out</b-dropdown-item>
            </div>
            <b-dropdown-item v-else href="/auth/login">Sign In</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>

<script>
export default {
  data() {
    return {
      auth_user: {},
      access_token: '',
      email: '',
      password: '',
      company_id: ''
    }
  },
  created() {
    this.getAuthUser();
  },
  watch: {
  // call again the method if the route changes
  '$route': 'getAuthUser'
  },
  methods: {
    async getAuthUser() {
      if(this.email && this.password) {
        fetch('http://ngauge.ml/api/v1/user/login', {
          method: 'POST', // or 'PUT'
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ email: this.email, password: this.password }),
        })
        .then(res => res.json())
        .then(data => {
          this.auth_user = data.user;
          this.access_token = data.access_token;
          this.company_id = data.user.company_id;
          // console.log(data)
        })
        .catch(err => console.log(err))
      } else {
        console.log("Please enter email and password")
      }
    }
  }
}
</script>
