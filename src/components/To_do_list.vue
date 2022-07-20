<template>
  <div class="container mt-5">
    <h2 class="text-center display-4 mt-5 mb-3">{{ ToDo }}</h2>

    <!-- Inputs -->
    <div class="d-flex">
      <input v-model="task" type="text" placeholder="Digite uma tarefa" class="form-control">
      <button @click="submitTask" class="btn btn-warning rounded-3">Enviar</button>
    </div>

    <!-- Tabela de tarefas -->

    <table class="table mt-5">
      <thead>
        <tr>
          <th scope="col">Tarefa</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(tasks, index) in tasks" :key="index">
          <td>
            <span :class="{'finished': tasks.status === 'finished'}"> {{ tasks.name }} </span>
          </td>
          <td style="width: 120px">
            <span @click="changeStatus(index)" class="pointer" :class="{'text-danger': tasks.status == 'to-do', 'text-warning': tasks.status == 'in progress', 
            'text-success': tasks.status == 'finished'}">
              {{ firstChangeUpper(tasks.status) }}
            </span>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>

  </div>  
</template>

<script>
  export default {
    name: 'ToDoList',
    props: {
      msg: String,
    },

    data() {
      return {
        ToDo: 'Minha lista de tarefas',
        task: '',
        editedTask: null,
        avaliableStatus: ['to-do','in progress', 'finished'],
        tasks: [
          {
            name: 'Enviar as depesas de Iguatu.',
            status: 'to-do'
          },
          {
            name: 'Solicitar os comprovantes dos abastecimentos.',
            status: 'in-progress'
          }
        ]
      }
    },
    methods: {
      submitTask(){
        console.log(this.task)
        if(this.task.length === 0) return;

        if(this.editedTask === null){
          this.tasks.push({
            name: this.task,
            status: 'to-do'
          });
        }else{
          this.tasks[this.editedTask].name = this.task;
          this.editedTask = null;
        }

        this.task = '';
      },

      deleteTask(index){
        this.tasks.splice(index, 1);
      },

      editTask(index){
        this.task = this.tasks[index].name;
        this.editedTask = index;
      },

      changeStatus(index){
        let newIndex = this.avaliableStatus.indexOf(this.tasks[index].status);
        if(++newIndex > 2 ) newIndex = 0;
        this.tasks[index].status = this.avaliableStatus[newIndex];
        
      },

      firstChangeUpper(str){
        return str.charAt(0).toUpperCase() + str.slice(1);
      },
    }
  }
</script>

<style scoped>
.container{
  width: 60%;
  background: rgba( 255, 255, 255, 0.2 );
  box-shadow: 0 8px 32px 0 rgba( 31, 38, 135, 0.37 );
  backdrop-filter: blur( 6.5px );
  -webkit-backdrop-filter: blur( 6.5px );
  border-radius: 10px;
  border: 1px solid rgba( 255, 255, 255, 0.18 );
}
.pointer{
  cursor: pointer;
}
.finished{
  text-decoration: line-through;
}
</style>

