<template>
    <body>
        <div class="container" id="app">
            <h2 style="text-align: center">ToDo App</h2>
            <input type="text" v-model="todo" id="name" class="input" placeholder="Please enter your ToDo" />
            <h4 v-if="isError==true">Please insert a value</h4>
            <input type="text" class="input" placeholder="Sr no." @keypress="validateNumber" v-model="priority" />
            <h4 v-if="isErrorNum">Please insert a number</h4>
            <input type="text" v-model="Desc_box" id="Description" class="input" placeholder="Description" />
            <input type="text" v-model="Cat_box" id="Category" class="input" placeholder="Category" />
            <br/>
            <input type="button" value="Add" id="btnClick" class="btn" 
            @click="storeToDo"/>
            <!-- <span id="error"></span> -->
            <div style="display: flex; justify-content: space-between;"> 
                <div><h3 style="padding-left: 10px;">ToDo List</h3></div>
                <div style="margin-top: 20px;">
                    <div class="filter-box">
                        <select class="form-control" v-model="selectedCategory" >
                          <!-- <option value="" selected disabled>Category</option> -->
                            <option value="" selected enabled >All </option>
                            <option
                                v-for="(cat, index) in dropDown"
                                :key="index"
                                style="background-color: white; color: black; font-weight: bold;">
                        {{ cat }}
                            </option>
                        </select>

                          <!-- <option v-for="(todo,index) in todos"  :key="index" style="background-color: white; color: black; font-weight: bold;">
                            {{ todo.cat }} -->
                  </div>
                </div>
        </div>
            
            <hr size="1" width="100%" color="white" />
            <ul id="box">
                <li v-for="(todo,index) in computed_items" :key="index" :class="{'strikeout': todo.isStrikedOff == true}">
                    <div>
                   {{ todo.seq }}
                   {{ todo.name }}
                    <div class="desc_display">
                        {{ todo.desc }} 
                    </div>
                    </div> 
                   

                    <button @click="deleteToDo(index)" class="remove">Remove</button>
                </li>
            </ul>
            <h3 style="padding-left:10px;">Removed List</h3>
            <hr size="1" width="100%" color="white">
            <ul>
                    <li v-for="(todo, index) in removedTodos" :key="index" >
                       <div class="strikeout">
                          {{ todo.seq }} .
                          {{ todo.name }}
                          <div class="desc_display">
                              {{ todo.desc }} 
                          </div>  
                       </div> 
                        <button class="remove-btn delete-btn" @click="clearTodo(index) ">Delete</button>
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
        priority: "",
        priorities: [],
        isErrorNum: false,
        removeTodo: "",
        removedTodos: [],
        Desc_box: "",
        // Cat: "",
        selectedCategory: ""
        };

    },

    methods: {
            validateNumber()
        {
         
          let keyCode = event.keyCode;
          if(keyCode < 48 || keyCode > 57)
          {
            event.preventDefault();
            this.isErrorNum = true
          }
          else{
            this.isErrorNum = false
          }
        },
            storeToDo() {
                if(this.todo!=""){
                 this.todos.push({
                  name: this.todo,
                  seq: Number(this.priority),
                  desc: this.Desc_box,
                  cat: this.Cat_box,
                  isStrikedOff:false,
                });
                // sort the this.todos array
              this.todos.sort( (a, b) => { return a.seq - b.seq })
              // sort array complete
                this.todo = "";
                this.priority = "";
                this.Desc_box = "";
                this.Cat_box = "";
                this.isError= false
            } else{
                this.isError=true
            }
        },
            deleteToDo(index){
                this.removedTodos.push(...this.todos.splice(index, 1));
                
            },
            clearTodo(index){
                this.removedTodos.splice(index, 1)
            }

    },
    computed: {
        computed_items: function () {
            if(this.selectedCategory!=="") {
                let filtertype = this.todos.filter((a)=>{
            if(a.cat === this.selectedCategory){
                return a;
            } 
        }) 
        return filtertype;
        } else {
          return this.todos;
        }
      },
        dropDown() {
        let arr= [];
        this.todos.forEach((e) => {
          arr.push(e.cat);
        });
        let unique = arr.filter((item, i, ar) => ar.indexOf(item) === i);
          return unique;
      }
    },

    mounted()
   {
     console.log('App mounted!');
     if (localStorage.getItem('todos')) this.todos = JSON.parse(localStorage.getItem('todos'));
     if (localStorage.getItem('removedTodos')) this.removedTodos = JSON.parse(localStorage.getItem('removedTodos'));
   },
   watch:
   {
     todos:
     {
       handler()
       {
         console.log('Todos changed!');
         localStorage.setItem('todos', JSON.stringify(this.todos));
       },
       deep: true,
     },
     removedTodos:
     {
       handler()
       {
         console.log('Todos changed!');
         localStorage.setItem('removedTodos', JSON.stringify(this.removedTodos));
       },
       deep: true,
     },
   },
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
    height: 1000px;
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
    margin: 10px;
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

h4 {
    margin-left: 10px;
    margin-top: 0px;
}

.strikeout{
    text-decoration: line-through;
}

.desc_display {
  color: rgb(192,192,192);
  justify-content: left;
}

.filter-box {
  margin-top: 22.5px;
}

.form-control {
  background-color: #11856c;
  color: white;
  width: 120px;
  border-radius: 5px;
}
</style>
