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
              <th scope="col" v-for="(value, key) in results[0]" :key="key">
                {{ key }}
              </th>
              <th>Action</th>
              <!-- <th scope="col">{{ index }}</th>
              <th scope="col">Name</th>
              <th scope="col">Username</th>
              <th scope="col">Email</th>
              <th scope="col">Phone</th>
              <th scope="col">Action</th> -->
            </tr>
          </thead>
          <tbody>
            <tr v-for="user in results" :key="user.id">
              <th scope="col">{{ user.id }}</th>
              <th scope="col" v-if="user.title">{{ user.title }}</th>
              <th scope="col" v-if="user.title">{{ user.body }}</th>
              <th scope="col" v-if="user.name">{{ user.name }}</th>
              <th scope="col" v-if="user.photos">
                <table class="table table-striped table-success">
                  <tr v-for="photo in user.photos.data" :key="photo.id">
                    <td>
                      <b-img left :src="photo.url" alt="Left image"></b-img>
                    </td>
                  </tr>
                </table>
              </th>
              <th scope="col" v-if="user.name">{{ user.username }}</th>
              <th scope="col" v-if="user.name">{{ user.email }}</th>
              <th scope="col" v-if="user.name">{{ user.phone }}</th>
              <th scope="col" v-if="user.name">
                <b-button
                  v-on:click="userInfo(user.id)"
                  v-b-modal.modal-1
                  variant="primary"
                  class="m-2"
                  >User Info</b-button
                >
                <b-button
                  v-on:click="UserPosts(user.id)"
                  variant="success"
                  class="m-2"
                  >User's Post</b-button
                >
                <b-button
                  v-on:click="UserAlbums(user.id)"
                  variant="info"
                  class="m-2"
                  >User's Albums</b-button
                >
                <b-button
                  v-on:click="UserCompany(user.id)"
                  variant="warning"
                  class="m-2"
                  v-b-modal.modal-2
                  >User's Company Info</b-button
                >
              </th>
              <th scope="col" v-if="user.title">
                <b-button
                  v-on:click="deletePost(user.id)"
                  variant="danger"
                  class="m-2"
                  >Delete Post</b-button
                >
                <b-button
                  variant="warning"
                  @click="toggleModalupdate(user)"
                  class="m-2"
                  >Update Post</b-button
                >
                <b-button @click="toggleModal" variant="primary" class="m-2"
                  >Create Post</b-button
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

      <b-modal id="modal-2" title="BootstrapVue">
        <div class="col-md-12">
          <table class="table table-borderless">
            <thead>
              <tr>
                <th scope="col">id :</th>
                <th scope="col">{{ userCompanyInfo.id }}</th>
              </tr>
              <tr>
                <th scope="col">Name :</th>
                <th scope="col">{{ userCompanyInfo.name }}</th>
              </tr>
              <tr>
                <th scope="col">Company :</th>
                <th scope="col">{{ userCompanyInfo.cname }}</th>
              </tr>
              <tr>
                <th scope="col">bs :</th>
                <th scope="col">{{ userCompanyInfo.bs }}</th>
              </tr>
              <tr>
                <th scope="col">catchPhrase :</th>
                <th scope="col">{{ userCompanyInfo.catchPhrase }}</th>
              </tr>
            </thead>
          </table>
        </div>
      </b-modal>
    </div>
    <div>
      <b-modal ref="my-modal" hide-footer title="Insert New Post">
        <div class="d-block text-center">
          <b-form-group
            label-cols="4"
            label-cols-lg="2"
            label-size="sm"
            label="POST"
            label-for="input-sm"
          >
            <b-form-input
              id="input-sm"
              placeholder="title"
              v-model="insertData.title"
              class="m-2"
              size="sm"
              required
            ></b-form-input>
            <b-form-input
              id="input-sm"
              placeholder="body"
              v-model="insertData.body"
              class="m-2"
              size="sm"
              required
            ></b-form-input>
          </b-form-group>
        </div>
        <b-button class="m-2" variant="outline-danger" block @click="hideModal"
          >Close Me</b-button
        >
        <b-button
          class="m-2"
          variant="outline-primary"
          block
          v-on:click="createPosts(insertData)"
          @click="toggleModal"
          >Insert Post</b-button
        >
      </b-modal>
    </div>
    <div>
      <b-modal ref="my-modal-2" hide-footer title="Insert New Post">
        <div class="d-block text-center">
          <b-form-group
            label-cols="4"
            label-cols-lg="2"
            label-size="sm"
            label="POST"
            label-for="input-sm"
          >
            <b-form-input
              id="input-sm"
              placeholder="id"
              v-model="postUpdateResult.id"
              class="m-2"
              size="sm"
              readonly
            ></b-form-input>
            <b-form-input
              id="input-sm"
              placeholder="title"
              v-model="postUpdateResult.title"
              class="m-2"
              size="sm"
              required
            ></b-form-input>
            <b-form-input
              id="input-sm"
              placeholder="body"
              v-model="postUpdateResult.body"
              class="m-2"
              size="sm"
              required
            ></b-form-input>
          </b-form-group>
        </div>
        <b-button class="m-2" variant="outline-danger" block @click="hideModal"
          >Close Me</b-button
        >
        <b-button
          class="m-2"
          variant="outline-primary"
          block
          v-on:click="upadatePosts(postUpdateResult)"
          @click="toggleModalupdate"
          >Update Post</b-button
        >
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
      userCompanyInfo: {
        id: "",
        name: "",
        company: "",
        cname: "",
        catchPhrase: "",
        bs: "",
      },
      userInfoResults: {
        uid: "",
        name: "",
        username: "",
        email: "",
        address: "",
        website: "",
        company: "",
      },
      UserPostsResults: [],
      postUpdateResult: {
        id: "",
        title: "",
        body: "",
      },

      insertData: {
        title: "",
        body: "",
      },
      id: "",
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
          query: `query(
                          $id: ID!,
                    ){
                    user(id:$id) {
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
          variables: { id: uid },
        }),
      })
        .then((response) => response.json())
        .then((data) => {
          console.log(data["data"].user);
          this.userInfoResults = data["data"].user;
          this.userInfoResults.address = data["data"].user.address.street;
          this.userInfoResults.company = data["data"].user.company.name;
        });
    },
    UserPosts(uid) {
      console.log(uid);
      fetch("https://graphqlzero.almansi.me/api", {
        method: "POST",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({
          query: `query($id: ID!){
              user(id: $id) {
                posts {
                  data {
                    id
                    title
                    body
                  }
                }
              }
            }`,
          variables: { id: uid },
        }),
      })
        .then((response) => response.json())
        .then((data) => {
          this.results = data["data"].user.posts.data;
        });
    },
    UserAlbums(aid) {
      console.log(aid);
      fetch("https://graphqlzero.almansi.me/api", {
        method: "POST",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({
          query: `query ($id: ID!){
                  user(id: $id) {
                    albums{
                      data{
                        id
                        photos{
                          data{
                            thumbnailUrl
                            url
                          }
                        }
                      }
                    }
                  }
                }`,
          variables: { id: aid },
        }),
      })
        .then((response) => response.json())
        .then((data) => {
          console.log(data["data"].user.albums.data);
          this.results = data["data"].user.albums.data;
        });
    },
    deletePost(pid) {
      console.log(pid);
      // this.id = pid;

      fetch("https://graphqlzero.almansi.me/api", {
        method: "POST",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({
          query: `
            mutation (
                  $id: ID!
                ) {
                  deletePost(id: $id)
            }`,
          variables: { id: pid },
        }),
      })
        .then((response) => response.json())
        .then((data) => {
          console.log(data);
          // this.results = data["data"].user.posts.data;
        });
    },
    upadatePosts(postUpdateResult) {
      console.log(this.postUpdateResult.id);
      fetch("https://graphqlzero.almansi.me/api", {
        method: "POST",
        headers: {
          "content-type": "application/json",
          Accept: "application/json",
        },
        body: JSON.stringify({
          query: `mutation (
                      $id: ID!,
                      $postUpdateResultData: UpdatePostInput!
                    ) {
                      updatePost(id: $id, input: $postUpdateResultData) {
                        id
                        title
                        body
                      }
                    }`,
          variables: {
            id: postUpdateResult.id,
            postUpdateResultData: {
              title: postUpdateResult.title,
              body: postUpdateResult.body,
            },
          },
        }),
      })
        .then((response) => response.json())
        .then((data) => {
          console.log(data["data"]);
          // this.results = data["data"].user.posts.data;
        });
    },
    createPosts(insertData) {
      console.log(insertData);
      this.insertData = insertData;
      fetch("https://graphqlzero.almansi.me/api", {
        method: "POST",
        headers: {
          "content-type": "application/json",
          Accept: "application/json",
        },
        body: JSON.stringify({
          query: `mutation(
                $insertData: CreatePostInput!
              ) {
                createPost(input: $insertData) {
                  id
                  title
                  body
                }
              }`,
          variables: { insertData: insertData },
        }),
      })
        .then((response) => response.json())
        .then((data) => {
          console.log(data["data"]);
          // this.results = data["data"].user.posts.data;
        });
    },
    UserCompany(uid) {
      console.log(uid);
      fetch("https://graphqlzero.almansi.me/api", {
        method: "POST",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({
          query: `query(
                          $id: ID!,
                    ){
                    user(id:$id){
                          name
                          id
                          company{
                            name
                            catchPhrase
                            bs
                          }
                        }
                      }`,
          variables: { id: uid },
        }),
      })
        .then((response) => response.json())
        .then((data) => {
          console.log(data["data"].user);
          this.userCompanyInfo = data["data"].user;
          this.userCompanyInfo.cname = data["data"].user.company.name;
          this.userCompanyInfo.bs = data["data"].user.company.bs;
          this.userCompanyInfo.catchPhrase =
            data["data"].user.company.catchPhrase;
        });
    },
    showModal() {
      this.$refs["my-modal"].show();
    },
    hideModal() {
      this.$refs["my-modal"].hide();
    },
    toggleModal() {
      // We pass the ID of the button that we want to return focus to
      // when the modal has hidden
      this.$refs["my-modal"].toggle("#toggle-btn");
    },
    toggleModalupdate(user) {
      this.postUpdateResult.id = user.id;
      this.postUpdateResult.title = user.title;
      this.postUpdateResult.body = user.body;
      // We pass the ID of the button that we want to return focus to
      // when the modal has hidden
      this.$refs["my-modal-2"].toggle("#toggle-btn");
    },
  },
};
</script>
