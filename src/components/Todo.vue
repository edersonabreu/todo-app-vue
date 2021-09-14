<template>
  <div class="container">
        <h2 class="text-center mt-5">Vue Todo App</h2>
        <div class="d-flex">
            <input v-model="task" type="text" placeholder="Digite uma nova tarefa..." class="form-control">
            <button @click="submitTask" class="btn btn-success rounded-0">Enviar</button>
        </div>
        <table class="table table-bordered mt-5">
            <thead>
                <tr>
                <th scope="col">A fazer...</th>
                <th scope="col">Status</th>
                <th scope="col" class="text-center">#</th>
                <th scope="col" class="text-center">#</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(task, index) in tasks" :key="index">
                    <th><span :class="{'finish': task.status === 'finalizado'}">{{task.name}}</span> </th>
                    <td style="width: 220px">
                        <span @click="changeStatus(index)" class="pointer font-bold" 
                            :class="{'text-danger': task.status === 'a fazer',
                            'text-warning': task.status === 'em progresso',
                            'text-success': task.status === 'finalizado'}">
                            {{firstCharUpper(task.status)}}
                        </span> 
                    </td>
                    <td>
                        <div class="text-center">
                            <button class="btn btn-primary" @click="editTask(index)">
                                Editar    
                            </button>
                        </div>
                    </td>
                    <td>
                        <div class="text-center">
                            <button class="btn btn-danger" @click="deleteTask(index)">
                                Excluir
                            </button>
                        </div>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
 
</template>

<script>
export default {
    data(){
        return{
            task: '',
            editedTask: null,
            avaliableStatus: ['a fazer', 'em progressso', 'finalizado'],

            tasks: [
                {
                    name: 'Vender discos velhos',
                    status: 'em progresso'
                },
                 {
                    name: 'Ir a lavanderia no sábado',
                    status: 'a fazer'
                },
            ]
        }
    },

    methods: {
        submitTask(){
            if(this.task.length === 0) return;

            if(this.editedTask === null){
                this.tasks.push({
                    name: this.task,
                    status: 'a fazer'
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
            this.editTask = index;
        },

        changeStatus(index){
            let newIndex = this.avaliableStatus.indexOf(this.tasks[index].status);
            if(++newIndex > 2) newIndex = 0;
            this.tasks[index].status = this.avaliableStatus[newIndex];
        },

        firstCharUpper(str){
            return str.charAt(0).toUpperCase() + str.slice(1);
        }
    }
}
</script>

<style scoped>
    *{
        font-family: 'Roboto', sans-serif;
    }
    .pointer{
        cursor: pointer;
    }
    .finish{
        text-decoration: line-through;
    }

    .font-bold{
        font-weight: bold;
    }

    .text-success{
        color: green;
    }
</style>
