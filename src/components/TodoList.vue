<template>
  <div class="container" >
    <div class="tambah-data">
      <div class="form-tambah">
      <h2>Tugas Kuliah</h2>
      <form @submit.prevent="addData">
        <input type="text" placeholder="Nama Tugas" v-model="iTugas"/>
        <br/>
        <input type="text" placeholder="Mata Kuliah" v-model="iMatkul"/>
        <br/>
        <input type="text" placeholder="Pengumpulan" v-model="iPengumpulan"/>
        <br/>
        <input type="text" placeholder="Deskripsi" v-model="iDeskripsi"/>
        <br />
        <button class="btn-add" @click="addData">TAMBAH</button>
      </form>
      </div>
    </div>
    <div class="todo-list">
      <div class="item" v-for="(item, index) in todoList" :key="index">
        <div class="card-todo">
          <div class="col-8">
          <h2 style="text-align: center">{{item.tugas}}</h2>
          <p style="text-align: center">{{item.matkul}} | Due Date : {{item.pengumpulan}}</p>
          <p style="text-align: center">{{item.deskripsi}}</p>
          </div>
          <div class="col-4">
            <button class="btn-remove" @click="removeData(item)" >HAPUS</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    iTugas: '',
    iMatkul: '',
    iPengumpulan: '',
    iDeskripsi: '',
    todoList: [],
  }),
  created() {
    this.loadlocalStorage();
  },
  watch: {
    todoList() {
      localStorage.setItem('todoList', JSON.stringify(this.todoList));
    },
  },
  methods: {
    addData() {
      if (this.iTugas !== '') {
        const date = new Date();
        const dataTask = {
          id: date.getTime(),
          tugas: this.iTugas,
          matkul: this.iMatkul,
          pengumpulan: this.iPengumpulan,
          deskripsi: this.iDeskripsi,
          complete: false,
          show: false,
        };
        this.todoList.push(dataTask);
      }
      this.iTugas = '';
      this.iMatkul = '';
      this.iPengumpulan = '';
      this.iDeskripsi = '';
    },
    removeData(item) {
      const index = this.todoList.findIndex((Element) => Element.id === item.id);
      this.todoList.splice(index, 1);
    },
    loadlocalStorage() {
      const ls = JSON.parse(localStorage.getItem('todoList'));
      if (ls == null) {
        return;
      }
      this.todoList = ls;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  margin: 10px;
}
.tambah-data {
  width: 550px;
  text-align: center
}
.form-tambah {
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
  padding-top: 10px;
}
.todo-list {
  width: 750px;
}
input[type=text] {
  height: 30px;
  margin: 5px;
  padding: 5px;
  cursor: pointer;
  transition: 0.3s;
  width: 450px;
}
input[type=text]:focus {
  border: 2px solid dodgerblue;
  border-radius: 0px;
}
.btn-add{
    background-color: transparent;
    color:#4e9af1;
    border: 2px solid#4e9af1;
    border-radius:2em;
    text-align: center;
    margin: 5px 2px;
    padding: 10px 15px;
    cursor: pointer;
    transition: 0.3s;
    margin: 20px;
    width: 200px;
}
.btn-add:hover{
    background-color:#4e9af1;
    color: white;
}
.card-todo {
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
    transition: 0.3s;
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-auto-rows: minmax(10px, auto);
    margin-bottom: 20px;
    margin-bottom: 20px;
    background-color: #f5f5f5;
}
.col-8 {
  padding-left: 10px;
  width: 600px;
}
.card-todo:hover {
    box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2);
}
.btn-remove{
    background-color: transparent;
    color:red;
    border: 2px solid red;
    border-radius:2em;
    text-align: center;
    margin: 5px 2px;
    padding: 10px 15px;
    cursor: pointer;
    transition: 0.3s;
    margin-top: 50px;
}
.btn-remove:hover{
    background-color: red;
    color: white;
}
</style>