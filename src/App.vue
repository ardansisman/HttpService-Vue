<template>
  <div class="container">
    <div class="row">
      <div class="col-md-6 col-md-offset-3">
        <h3>Vue-Resource</h3>
        <div class="form-group">
          <input type="text" class="form-control" v-model="userName" />
        </div>
        <button class="btn btn-primary" @click="saveUser">Kaydet</button>
        <button class="btn btn-success" @click="getUsers">
          Verileri Getir
        </button>
        <hr />
        <ul class="list-group">
          <li
            v-for="user in userList"
            :key="user.userName"
            class="list-group-item"
          >
            <span> {{ user.data.userName }} </span>
            <button class="btn btn-xs btn-danger" @click="deleteUser(user.key)">
              Sil
            </button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      userName: null,
      userList: [],
      resource: {},
    };
  },
  methods: {
    saveUser() {
      // this.$http.post("users.json", { userName: this.userName })
      //   .then((response) => {
      //     console.log(response);
      //     this.userName = null;
      //     this.getUsers();
      //   });
      //this.$resource("users.json").save({},{userName : this.userName})
      this.resource.kaydet({},{userName : this.userName});
      this.getUsers();
    },
    getUsers() {
      // this.$resource("users.json").get()
      this.$http
        .get("users.json")
        .then((response) => {
          // this.userList=[];
          // let data = response.data;
          // for(let key in data){
          //   console.log(data[key]);
          //   this.userList.push(data[key]);
          // }
          return response.json();
        })
        .then((data) => {
          this.userList = [];
          for (let key in data.userList) {
            this.userList.push({
              key: key,
              data: data.userList[key],
            });
            console.log(data);
          }
        });
    },
    deleteUser(userKey) {
      this.$http.delete("users/" + userKey + ".json").then((response) => {
        console.log(response);
        this.getUsers();
      });
    },
  },
  created() {
    const customActions = {
      kaydet: { method: "POST", url: "users.json" },
    };
    this.resource = this.$resource("users.json", {}, customActions);
  },
};
</script>

<style>
</style>
