<template>
    <body>
        <div class="container" id="app">
            <h2 style="text-align: center">ToDo App</h2>
            <input type="text" v-model="todo" id="name" class="input" placeholder="Please enter your ToDo" />
            <h4 v-if="isError==true">Please insert a value</h4>
            <input type="button" value="Add" id="btnClick" class="btn" 
            @click="storeToDo"/>
            <!-- <span id="error"></span> -->
            <h3 style="padding-left: 10px;">ToDo List</h3>
            <hr size="1" width="100%" color="white" />
            <ul id="box">
                <li v-for="(todo,index) in todos" :key="index">
                    {{ todo }}

                    <button @click="deleteToDo(index)" class="remove">Remove</button>
                </li>
            </ul>
        </div>
<!-- 
        <script src="Vue JS/vue.js"></script>
        <script src="scripts.js"></script> -->
    </body>
</template>

<script>
export default {
    name: 'app',

    data() {
        return{
        todo: '',
        todos: [],
        isError: false,
        }

    },

    methods: {
            storeToDo() {
                if(this.todo!=""){
                this.todos.push(this.todo)
                this.todo=''
                this.isError=false
            } else{
                this.isError=true
            }
        },
            deleteToDo(index){
                this.todos.splice(index, 1)
            }
    }
}
</script>

<style>
body{
    font-family: "Poppins", sans-serif;
}
.container{
    padding: 10px;
    border: 3px solid #11856c;
    width: 40%;
    height: 500px;
    margin: auto;
    background-color: #040101;
    color: #11856c;
}
.container .input{
    width: 65%;
    height: 25px;
    margin: 10px;
}
.container .btn{
    width: 20%;
    height: 31px;
    background-color: #11856c;
}

.btn:hover {
    cursor: pointer;
    background-color: white;
    transition-duration: 1000ms;
}
.container ul{
    list-style: none;
    padding: 0px;
}
.container li{
    border: 1.5px solid #11856c;
    margin-bottom: 10px;
    padding: 5px;
    color: white;
    display: flex;
    justify-content: space-between;
}

/* .container #error{
    color: red;
    font-size: 14px;
    padding-left: 10px;
} */

/* Remove and Add Event */

.remove {
    /* margin-left: 60%; */
    border: 1px solid black;
    border-radius: 5px;
    background-color: #11856c;
}

.remove:hover {
    cursor: pointer;
}
</style>
