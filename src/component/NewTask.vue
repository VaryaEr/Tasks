<template>
  <v-content id="all_tasks">
    <v-card width="100vw" class="mx-auto mt-9 all-window">
      <v-card-title>
        <p>Новая задача</p>
        <v-spacer></v-spacer>
        <v-btn text color="red" v-on:click="closeNewTask">Закрыть</v-btn>
      </v-card-title>

        <v-form
          ref="form"
          class="pa-4 pt-1"
          v-model="isValid"
        >
        <v-text-field required label="Название задачи"
                      :rules="[v => !!v || 'Название задачи обязательно!']"
                      v-model="tittle"
                      id="tittle"
        />
        <v-textarea
          label="Описание задачи"
          auto-grow
          v-model="description"
          id="description"
        />
          <v-menu
            v-model="fromDateMenu"
            :close-on-content-click="false"
            lazy
            transition="scale-transition"
            offset-y
          >
            <template v-slot:activator="{ on }">
              <v-text-field
                required
                :rules="[v => !!v || 'Выбор даты обязателен!']"
                label="Выберите дату"
                readonly
                :value="fromDateDisp"
                v-on="on"
                id="data"
              ></v-text-field>
            </template>
            <v-date-picker
              locale="en-in"
              v-model="fromDateVal"
              no-title
              @input="fromDateMenu = false"
            ></v-date-picker>
          </v-menu>
        </v-form>

      <v-divider></v-divider>

      <v-card-actions>
        <v-btn color="info"
               :disabled="!isValid"
               v-on:click="addNew()"
               id="new-task"
        >Создать</v-btn>
      </v-card-actions>
    </v-card>
  </v-content>
</template>
<style scoped>
#all_tasks{
  position: absolute;
  top: 0;
  padding: 10vw!important;
  display: none;
}
.all-window{
  margin: 0!important;
}
#new-task{
  width: 30%;
}
</style>
<script>
export default {
  name: "NewTask",
  data() {
    return {
      fromDateMenu: false,
      fromDateVal: null,
      tittle: null,
      description: null,
      isValid: true
    };
  },
  computed: {
    fromDateDisp() {
      return this.fromDateVal;
      // format date, apply validations, etc. Example below.
      // return this.fromDateVal ? this.formatDate(this.fromDateVal) : "";
    },
  },
  methods: {
    addNew: function () {
      let numberTasks = 0
      if (localStorage.getItem("numberTasks") == 'undefined' || localStorage.getItem("numberTasks") == null ){
        numberTasks = 1
        localStorage.setItem("numberTasks", String(numberTasks))
      }else{
        numberTasks = parseInt( localStorage.getItem("numberTasks") ) + 1
        localStorage.setItem("numberTasks", String(numberTasks))
      }
      let newTask = {
        tittle: document.getElementById('tittle').value,
        description: document.getElementById('description').value,
        data: document.getElementById('data').value,
      }
      localStorage.setItem(String(numberTasks), JSON.stringify(newTask))
      window.location.reload()

    },
    closeNewTask: function (){
      window.location.reload()
    }
  }
}
</script>

<style scoped>

</style>
