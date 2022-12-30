
<template>
  <div class="container">
    <div class="row">
      <div class="col-4">
        <h1>Vložit zaměstnance</h1>
        <br/>
          <form @submit.prevent="save">
            <div class="form-group">
                <label for="name">Jméno zaměstnance</label>
                <input type="text" v-model="employee.name" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" placeholder="">
            </div>
            <div class="form-group">
                <label for="adress">Adresa zaměstnance</label>
                <input type="text" v-model="employee.address" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" placeholder="">
            </div>
            <div class="form-group">
                <label for="telefon">Telefon zaměstnance</label>
                <input type="text" v-model="employee.mobile" class="form-control" id="exampleInputPassword1" placeholder="">
            </div>
            <br/>
            <button type="submit" class="btn btn-primary">ULOŽIT</button>
          </form>
      </div>
      <div class="col-8">
        <h1>Seznam zaměstnanců</h1>
          <table class="table table-striped">
              <thead>
              <tr>
                <th scope="col">Jméno</th>
                <th scope="col">Adresa</th>
                <th scope="col">Telefon</th>
                <th scope="col">Možnosti</th>
              </tr>
              </thead>
                <tbody>
                  <tr v-for="employee in result" v-bind:key="employee.id">
                    <td>{{employee.name}}</td>
                    <td>{{employee.address}}</td>
                    <td>{{employee.mobile}}</td>
                    <td>
                      <button type="button" class="btn btn-warning" @click="edit(employee)" >Upravit</button>
                      <button type="button" class="btn btn-danger" @click="remove(employee)" >Vymazat</button>
                    </td>
                  </tr>
                </tbody>
          </table>
    </div>
  </div>
</div>
</template>

<script>

import Vue from 'vue'
import axios from 'axios'

Vue.use(axios)

export default {
  name: 'EmployeeView',
  data () {
    return {
      result: {},
      employee: {
        id: '',
        name: '',
        address: '',
        mobile: ''

      }
    }
  },
  created () {
    this.EmployeeLoad()
  },

  methods: {
    EmployeeLoad () {
      var page = 'http://127.0.0.1:8000/api/employees'
      axios.get(page)
        .then(
          ({data}) => {
            console.log(data)
            this.result = data
          }
        )
    },
    save () {
      if (this.employee.id == '') {
        this.saveData()
        this.EmployeeLoad()
      } else {
        this.updateData()
        this.EmployeeLoad()
      }
    },
    saveData () {
      axios.post('http://127.0.0.1:8000/api/save', this.employee)
        .then(
          ({data}) => {
            alert('Zaměstnanec uložen.')
            this.EmployeeLoad()
          }
        )
    },
    edit (employee) {
      this.employee = employee
    },
    updateData () {
      var editrecords = 'http://127.0.0.1:8000/api/update/' + this.employee.id
      axios.put(editrecords, this.employee)
        .then(
          ({data}) => {
            this.employee.name = ''
            this.employee.address = ''
            this.employee.mobile = ''
            this.id = ''
            alert('Updated!!!')

            this.EmployeeLoad()
          }
        )
    },
    remove (employee) {
      var url = `http://127.0.0.1:8000/api/delete/${employee.id}`
      axios.delete(url)
      alert('Deleteddd')
      this.EmployeeLoad()
    }

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 .col-4{
    background: rgb(58, 109, 220);
    padding: 15px;
    border-radius: 25px;

  }
  .col-8{
    background: rgb(9, 222, 122);
    padding: 15px;
    border-radius: 25px;
  }
  .form-control{

    text-align: center;
  }
</style>
