<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <h2>A Form to Create a User</h2>
    <div class="row mt-5">
      <div class="col-md-8 offset-md-1">
        <form>
          <div class="input-group mb-3">
            <input
              v-model="newUserName"
              type="text"
              placeholder="Enter name of User"
              class="form-control"
            />
            <div class="input-group-append">
              <button type="button" @click="saveUser" class="btn btn-secondary">
                Submit
              </button>
            </div>
          </div>
        </form>
      </div>
      <div class="col-md-8">
        <ul class="list-group">
          <li class="list-group-item" v-for="user in users" :key="user.id">
            {{ user.name }}
            <div class="span"><span @click="delUser(user)">X</span></div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import axios from "axios";
export default {
  name: "Home",
  components: {
    // HelloWorld
  },
  data() {
    return {
      users: [],
      newUserName: null
    };
  },
  async created() {
    let collectUsers = await axios.get(
      "https://jsonplaceholder.typicode.com/users"
    );
    console.log(collectUsers);
    this.users = collectUsers.data;
  },
  methods: {
    saveUser() {
      if (!this.existingUser(this.newUserName)) {
        let lastUserIndex = this.users.length - 1;
        let lastUser = this.users[lastUserIndex];

        let newUserid = lastUser.id + 1;

        let newuser = {
          id: newUserid,
          name: this.newUserName
        };

        this.users.push(newuser);
        this.newUserName = "";
      }
    },
    existingUser(newUserName) {
      let duplicate = false;

      this.users.forEach(user => {
        if (user.name === newUserName) {
          duplicate = true;
          alert(
            "This User " + this.newUserName + " already exits in this Database"
          );

          this.newUserName = null;
          this.users.pop(newuser);
        }
      });
    },

    delUser(user) {
      this.users = this.users.filter(person => person.id !== user.id);
      console.log(user.id);
    }
  }
};
</script>

<style scoped>
span {
  color: red;
  float: right;
  cursor: pointer;
}
.span {
  width: 20px;
  float: right;
}
h2 {
  background-color: #555555;
  padding-top: 20px;
  padding-bottom: 20px;
  color: white;
  font-size: 40px;
}
.span:hover {
  border: 1px solid red;

  background-color: red;
}
span:hover {
  color: white;
  padding-right: 3px;
}
ul {
  margin-left: 250px;
  box-shadow: 5px 10px #f0efef;
}
input {
  margin-left: 120px;
}
</style>
