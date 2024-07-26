<template>
  <div id="app">
    <h1>Пользователи</h1>
    <UsersTable 
      :usersData="usersData"
    />
    <button 
      class="primary-button"
      @click="formVisibility = true"
    >
      Добавить
    </button>
    <AddUserForm 
      v-show="formVisibility"
      :usersData="usersData"
      :parentsList="parentsList"
      @add-user="addUser"
      @close-form="closeForm"
    />
  </div>
</template>

<script>
import UsersTable from './components/UsersTable.vue';
import AddUserForm from './components/AddUserForm.vue';
import './assets/style/variables.css';

export default {
  name: 'App',
  components: {
    UsersTable,
    AddUserForm
  },
  data() {
    return {
      // пользователи в виде вложенного списка
      usersData: [],
      // пользователи для выбора начальника
      parentsList: [],
      id: 1,
      formVisibility: false
    }
  },
  created() {
    this.loadData();
  },
  methods: {
    addUser(user, parent) {
      const newUser = {
        id: this.id,
        name: user.name,
        phone: user.phone,
        children: []
      };
      this.addUserToParentList(newUser);
      this.addUserToUsersData(parent, newUser)
      this.closeForm();
      this.saveData();
      this.id++;
    },
    addUserToUsersData(parent, user) {
      if(parent) {
        const parentUser = this.findParentById(this.usersData, parent);
        if(parentUser) {
          parentUser.children.push(user);
        }
      } else {
        this.usersData.push(user);
      }
    },
    addUserToParentList(user) {
      this.parentsList.push(user);
    },
    findParentById(users, id) {
      for (const user of users) {
        if (user.id === id) {
          return user;
        }
        if (user.children && user.children.length > 0) {
          const found = this.findParentById(user.children, id);
          if (found) {
            return found;
          }
        }
      }
      return null;
    },
    closeForm() {
      this.formVisibility = false;
    },
    loadData() {
      this.usersData = JSON.parse(localStorage.getItem('usersData')) || [];
      this.parentsList = JSON.parse(localStorage.getItem('parentList')) || [];
      this.id = JSON.parse(localStorage.getItem('lastId')) || 1;
    },
    saveData() {
      localStorage.setItem('usersData', JSON.stringify(this.usersData));
      localStorage.setItem('parentList', JSON.stringify(this.parentsList));
      localStorage.setItem('lastId', JSON.stringify(this.id));
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.primary-button {
  position: fixed;
  bottom: 10px;
  right: 10px;
  background-color: var(--primary-color);
  border: none;
  border-radius: var(--border-radius);
  padding: 10px 20px;
  color: #fff;
  text-transform: uppercase;
  font-weight: 600;
  z-index: 100;
  cursor: pointer;
}
</style>
