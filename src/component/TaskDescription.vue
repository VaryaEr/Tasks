<template>
  <v-col class=" position-abs" md="100%">

    <v-card>

      <v-card-title>
        <h3 class="text-h4">{{ task_list.title }}</h3>
        <v-spacer></v-spacer>
        <v-btn text color="red" @click='$emit("customClick", false)'>Назад</v-btn>
      </v-card-title>
      <v-card-text>
        <p class="text-h5">Описание задачи: {{ task_list.description }}</p>
        <p class="text-h5">Дата выполнения: {{ task_list.data }}</p>
      </v-card-text>
    </v-card>
  </v-col>
</template>
<style scoped>
.position-abs{
  position: absolute;
  top: 20%;
}
</style>
<script>
export default {
  name: "TaskDescription",
  data: () => ({
    task_list:
      {
        title: "",
        description: "",
        data: ""
      },
  }),
  props: ['idForProps'],
  created() {
        if (this.idForProps != 0){
          var objectTasks = JSON.parse(localStorage.getItem(String(this.idForProps)))
          this.task_list.data = objectTasks.data
          this.$set( this.task_list, 'title', objectTasks.tittle )
          if(objectTasks.description.length == 0){
            this.task_list.description = "Без описания"
          }else{
            this.task_list.description = objectTasks.description
          }
          console.log(this.task_list.title)
        }
  },

}
</script>

<style scoped>

</style>
