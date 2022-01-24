<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <div class="block-contacts" id="users">
          <!-- Button trigger modal -->
<button class="btn btn-primary btn__add-contact" @click="showModal">Добавить</button>
          <form v-if="showModalPage">
            <div class="form-control">
              <label>Имя</label>
              <input type="name" id="name" v-model="user.name" placeholder="Богдан">
            </div>
            <div class="form-control">
              <label>Номер телефона</label>
              <input type="number" id="number" v-model="user.number" placeholder="+79250774157">
            </div>
            <div class="form-control">
              <label>Начальник</label>
              <select size="1" id="rank" v-model="user.rank">
                <option v-bind:value="user.name" v-for="user in contacts" v-bind:key="user.id">{{user.name}}</option>
              </select>
            </div>
            <div class="form-control">
              <button type="submit" @click="addToList" class="btn btn-success">Добавить пользователя</button>
            </div>
            <hr>
          </form>
          <table class="block-contacts__table">
            <tr>
              <td><strong>Имя</strong></td>
              <td><strong>Телефон</strong></td>
            </tr>
            <tr v-for="user in contacts" v-bind:key="user.id">
              <td>{{user.name}}</td>
              <td>{{user.number}}</td>
            </tr>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'users',
  data () {
    return {
      showModalPage: false,
      user: {
        name: '',
        number: '',
        rank: ''
      },
      contacts: []
    }
  },
  methods: {
    showModal () {
      this.showModalPage = !this.showModalPage
    },
    addToList () {
      const id = (Object.keys(localStorage).length + 1) * 5
      localStorage.setItem(id, JSON.stringify(this.user))
    }
  },
  created: function () {
    this.contacts = []
    if (Object.keys(localStorage).length > 0) {
      const localstorageKeys = Object.keys(localStorage).sort()
      // eslint-disable-next-line no-extend-native
      Array.prototype.insert = function (index, value) {
        this.splice(index, 0, value)
      }
      for (let i = 0; i < localstorageKeys.length - 1; i++) {
        let localstorageValue = JSON.parse(localStorage.getItem(localstorageKeys[i]))
        this.contacts.push(localstorageValue)
      }
    }
    return this.contacts
  }
}
</script>
<style>
.block-contacts{
  width: 200px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
}
.btn__add-contact{
    margin: 10px;
}
.block-contacts__table{
  width: 350px;
}
.block-contacts__table td{
  border: 2px solid grey;
  padding: 5px;
}
.rank{
  width: 150px;
  float: right;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  margin-left: 5px;
}
.rank td{
  margin-left: 5px;
  width: 100px;
}
</style>
