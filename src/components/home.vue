<template>
  <div>
    <h2>GraphQL Play Ground</h2>
    <!-- <h3>{{ data }}</h3> -->
    <b-button class="w-25" v-on:click="data = showData()" variant="success"
      >Show All Users</b-button
    >

    <div class="row mt-4">
      <div class="col-md-10 offset-1 table-responsive">
        <table class="table table-striped table-success">
          <thead>
            <tr>
              <th scope="col">id</th>
              <th scope="col">Name</th>
              <th scope="col">Username</th>
              <th scope="col">Email</th>
              <th scope="col">Phone</th>
              <th scope="col">Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="user in results" :key="user.id">
              <th scope="col">{{ user.id }}</th>
              <th scope="col">{{ user.name }}</th>
              <th scope="col">{{ user.username }}</th>
              <th scope="col">{{ user.phone }}</th>
              <th scope="col">{{ user.email }}</th>
              <th scope="col">
                <b-button
                  v-on:click="userInfo(user.id)"
                  v-b-modal.modal-1
                  variant="primary"
                  >User Info</b-button
                >
              </th>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <div>
      <b-modal id="modal-1" title="BootstrapVue">
        <div class="col-md-12">
          <table class="table table-borderless">
            <thead>
              <tr>
                <th scope="col">id</th>
                <th scope="col">{{ userInfoResults.id }}</th>
              </tr>
              <tr>
                <th scope="col">name</th>
                <th scope="col">{{ userInfoResults.name }}</th>
              </tr>
              <tr>
                <th scope="col">username</th>
                <th scope="col">{{ userInfoResults.username }}</th>
              </tr>
              <tr>
                <th scope="col">email</th>
                <th scope="col">{{ userInfoResults.email }}</th>
              </tr>
              <tr>
                <th scope="col">address</th>
                <th scope="col">{{ userInfoResults.address }}</th>
              </tr>
              <tr>
                <th scope="col">website</th>
                <th scope="col">{{ userInfoResults.website }}</th>
              </tr>
              <tr>
                <th scope="col">company</th>
                <th scope="col">{{ userInfoResults.company }}</th>
              </tr>
            </thead>
          </table>
        </div>
      </b-modal>
    </div>
  </div>
</template>
<script>
export default {
  name: "Home",
  props: {
    data: String,
  },
  data() {
    return {
      results: [],
      userInfoResults: [],
    };
  },
  methods: {
    showData() {
      fetch("https://graphqlzero.almansi.me/api", {
        method: "POST",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({
          query: `{
                    users{
                        data{
                        id
                        name
                        username
                        email
                        phone
                        }
                    }
                    }`,
        }),
      })
        .then((response) => response.json())
        .then((data) => {
          this.results = data["data"].users.data;
        });
    },
    userInfo(uid) {
      console.log(uid);
      fetch("https://graphqlzero.almansi.me/api", {
        method: "POST",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({
          query: `{
                user(id:5) {
                    id
                    name
                    username
                    email
                    address{
                    street
                    }
                    phone
                    website
                    company{
                    name
                    }
                    posts{
                    links{
                        first{
                        limit
                        }
                    }
                    }
                }
                            }`,
        }),
      })
        .then((response) => response.json())
        .then((data) => {
          console.log(data["data"].user);
          this.userInfoResults = data["data"].user;
        });
    },
  },
};
</script>