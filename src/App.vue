<template>
  <v-app>

    <v-app-bar
      app
      color="red"
    >
      <v-app-bar-title>
        <h3 class="text-h4 white--text cursor-pointer" v-on:click="mainPage">Задачник</h3>

      </v-app-bar-title>
      <v-spacer></v-spacer>
      <v-btn text color="white" class="text-h3" v-on:click="displayTasks()">+</v-btn>
    </v-app-bar>


    <v-main id="display-how">
      <p><v-text-field label="Поиск по дате" class="mini-input" color="blue" type="text" v-model="data" /></p>
      <v-col :data-number="item.id" class="data-founder" md="100%" v-for="item in filteredList.slice().reverse()" :key="item.id">
        <v-card v-on:click="descriptionTasks" @mouseover="item.showButton = true" @mouseleave="item.showButton = false">

          <v-card-title>
            <h4 class="text-h5">{{ item.title }}</h4>
            <v-spacer></v-spacer>
            <v-btn class="no-open" height="100%" text color="black" v-on:click="deleteTask" v-if="item.showButton">Удалить задачу</v-btn>
          </v-card-title>

          <v-card-text>
            <p class="text-body-1">{{ item.data }}</p>
          </v-card-text>

        </v-card>
      </v-col>

    </v-main>
    <new-task></new-task>
    <task-description v-bind:id-for-props.sync='idForProps' v-if="isCompany" @customClick="closeDesk"></task-description>
  </v-app>
</template>
<style scoped>
.mini-input{
  width: 20%!important;
  margin-left: 5%;
  margin-top: 3%;
}
.cursor-pointer{
  cursor: pointer;
}
</style>
<script>
import NewTask from './component/NewTask.vue'
import TaskDescription from "./component/TaskDescription";

export default {
  components: {
    NewTask,
    TaskDescription
  },
  name: 'App',
  data: () => ({
    data: '',
    item_list: [

    ],
    idForProps: 0
  }),
  props: {
    isCompany: { type: Boolean, default: false },
  },
  beforeMount() {
    let numbersTasks = Number(localStorage.getItem("numberTasks"))
    if (numbersTasks !== 0){
      for (let i = 1; i <= numbersTasks; i++) {
        var objectTasks = JSON.parse(localStorage.getItem(String(i)));
        if (objectTasks != undefined || objectTasks != null){

          this.item_list.push({
            id: Number(i),
            title: objectTasks.tittle,
            description: objectTasks.description,
            data: objectTasks.data,
            showButton: false
          })
        }

      }

    }
  },
  methods: {
    displayTasks: function () {
      document.getElementById("all_tasks").style.display = "block"
      document.getElementById("display-how").style.display = "none"
      this.isCompany = false
    },
    descriptionTasks: function (event) {
      if (!event.target.parentNode.classList.contains("no-open")){
        this.idForProps = event.target.closest('.data-founder').getAttribute('data-number')
        this.isCompany = true
        document.getElementById("display-how").style.display = "none"
        console.log(event.target)
      }

    },
    closeDesk: function (falseValue){
      this.isCompany = falseValue
      document.getElementById("display-how").style.display = "block"
    },
    deleteTask: function (event){
      if (confirm("Вы подтверждаете удаление?")) {
        let idOfTask = event.target.closest('.data-founder').getAttribute('data-number')
        localStorage.removeItem(idOfTask )
        window.location.reload()
      } else{
        return false
      }

    },
    mainPage: function (){
      window.location.reload()
    }
  },
  computed:{
    filteredList: function(){
      var comp = this.data;
      return this.item_list.filter(function (elem) {

        if(comp==='') return true;
        else return elem.data.indexOf(comp) > -1;
      })
    }
  }
}
</script>
