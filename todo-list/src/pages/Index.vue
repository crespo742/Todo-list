<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pas-sm bg-primary">
      <q-input 
        v-model="newTask"
        @keyup.enter="addTask"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="Ajouter une tache"
        dense>
        <template v-slot:append>
          <q-btn
          @click="addTask" 
          round 
          dense 
          flat 
          icon="add" />
        </template>
      </q-input>
    </div>


    <q-list 
      class="bg-white"
      separator
      bordered>
      <q-item 
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1' : task.done }"
        clickable
        v-ripple>
        <q-item-section avatar>
          <q-checkbox 
          v-model="task.done" 
          class="no-pointer-events"
          color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section
          v-if="task.done"
          side>
          <q-btn 
          @click.stop="deleteTask(index)"
          flat 
          round 
          dense
          color="primary" 
          icon="cdelete" />
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: [
        // {
        //   title: 'apprendre',
        //   done: false
        // },
        // {
        //   title: 'travailler',
        //   done: false
        // },
        // {
        //   title: 'et re apprendre',
        //   done: false
        // }
      ]
    }
  },
  methods: {
    deleteTask(index) {
      this.$q.dialog({
        title:'Confirmez',
        message:'Voulez vous vraiment supprimée cette tache ?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1)
        this.$q.notify('Tache supprimé')
      })
    },
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false
      })
      this.newTask = ''
    }
  }
}
</script>


<style lang="scss">
  .done{
    .q-item__label{
      text-decoration: line-through;
      color: #bbb;
    }
  }
</style>