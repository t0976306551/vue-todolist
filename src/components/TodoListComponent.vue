<template>
<div id="app" class="container mt-4">
  <div class="input-group mb-3">
    <div class="input-group-prepend">
      <span class="input-group-text" id="basic-addon1">待辦事項</span>
    </div>
    <input type="text" class="form-control" placeholder="準備要做的任務" v-model="newTodo"  @keyup.enter="insertData">
    <div class="input-group-append">
      <button class="btn btn-primary" type="button" @click="insertData">新增</button>
    </div>
  </div>
  <div class="card">

    <div style="padding-left: 20px;" class="row align-items-start" v-for="item in todos" :key="item.id">
      <div class="col-10">
        <p>{{item.name}}</p>
      </div>
      <div class="col">
        <button type="button" :id="item.id" class="btn btn-outline-primary" @click="getUpdateData(item.id,item.name)" data-bs-toggle="modal" data-bs-target="#exampleModal">修改</button>
        <button type="button" :id="item.id" class="btn btn-outline-dark" @click="deleteData(item.id)">刪除</button>
      </div>
    </div>

    <div class="card-footer d-flex justify-content-between" style="margin-top: 20px;">
      <span>目前有{{todos.length}}筆任務</span>
      <!-- <a href="#">清除所有任務</a> -->
    </div>
  </div>
</div>


 <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">修改資料</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <input v-model="updateTodo"/>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">關閉</button>
        <button type="button" class="btn btn-primary" @click="updateData">修改</button>
      </div>
    </div>
  </div>
</div>

</template>


<script>

export default {
  data() {
    return {
      count: 0,
      todos:[],
      newTodo:'',
      updateId:'',
      updateTodo:'',
    }
  },
  methods: {

    increment() {
      this.count++
    },
    getData() {
      fetch('http://localhost:3000/todo/', {
       method: 'GET',
      //  mode: 'cors',
       headers: {
        "Content-Type": 'application/json',
      },
     })
       .then((res) =>{
          return res.json().then((data) => {
                console.log(data.data);
                this.todos = data.data;


            }).catch((err) => {
                console.log(err);
            })
       })
       .catch((error) => {
         console.log(error);
       });
    },
    insertData(){
      // this.newTodo = "Vue POST Request Example";
      // console.log(this.newTodo);

      fetch('http://localhost:3000/todo/', {
        method: 'POST',
        headers: {
          "Content-Type": 'application/json',
        },
        body:JSON.stringify({ name: this.newTodo }),
     })
       .then((res) =>{
          return res.json().then((data) => {
                console.log(data.message);
                if(data.message == "success"){
                  window.location.reload();
                }


            }).catch((err) => {
                console.log(err);
            })
       })
       .catch((error) => {
         console.log(error);
       });
    },
    updateData(){
      fetch('http://localhost:3000/todo/', {
        method: 'PUT',
        headers: {
          "Content-Type": 'application/json',
        },
        body:JSON.stringify({ id:this.updateId,name: this.updateTodo }),
     })
       .then((res) =>{
          return res.json().then((data) => {

                if(data.message == "success"){
                  window.location.reload();
                }

            }).catch((err) => {
                console.log(err);
            })
       })
       .catch((error) => {
         console.log(error);
       });
    },
    deleteData(deleteId){
      fetch('http://localhost:3000/todo/', {
        method: 'DELETE',
        headers: {
          "Content-Type": 'application/json',
        },
        body:JSON.stringify({ id:deleteId }),
     })
       .then((res) =>{
          return res.json().then((data) => {
              if(data.message == "success"){
                window.location.reload();
              }

            }).catch((err) => {
                console.log(err);
            })
       })
       .catch((error) => {
         console.log(error);
       });
    },
    getUpdateData(id, todo){
        this.updateId = id;
        this.updateTodo = todo;
    }
  },
  mounted() {
    this.getData(),
    console.log(`The initial count is ${this.count}.`)
  }
}
</script>
<style scoped>
button {
  font-weight: bold;
}
</style>