<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        @keyup.enter="addTask" 
        filled 
        v-model="newTask" 
        placeholder="Add task"
        dense
        square
        bg-color="white"
        class="col"       
      >
        <template v-slot:append>
          <q-btn
            @click="addTask" 
            round 
            dense 
            flat 
            icon="add"
          />
        </template>
      </q-input>
    </div>    
    <q-list class="bg-white" separator bordered>
      <!--
        Rendering a <label> tag (notice tag="label")
        so QCheckboxes will respond to clicks on QItems to
        change Toggle state.
      -->
      <q-item         
        v-for="(task, index) in tasks" 
        :key="task.id" 
        clickable 
        @click="task.done = !task.done"
        :class="{'done bg-blue-1': task.done}"
        v-ripple >
        <q-item-section avatar>
          <q-checkbox v-model="task.done" class="no-pointer-events" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{task.title}}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(index)" 
            flat 
            round 
            size="sm" 
            color="primary" 
            icon="delete" 
            />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary"/>
      <div class="text-h5 text-primary text-center">No tasks</div>
    </div>
  </q-page>
</template>

<script>
export default {
  name: 'todo',
  data() {
    return {
      tasks: [
        // {id: 1, title: 'Buy programming book', done: true},
        // {id: 2, title: 'Read programming book', done: false},
        // {id: 3, title: 'Write a program', done: false}
      ],
      newTask: ''
    }
  },
  methods: {
    addTask() {
      this.tasks.unshift({
        title: this.newTask,
        done: false
      })
      this.newTask = ''
    },
    deleteTask(index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Really delete?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1)
        this.$q.notify('Task deleted!')
      })      
    }
  }
}
</script>

<style lang="scss">
  .done {
    .q-item__label {
      text-decoration: line-through;
      color: #bbb;
    }
  }
  .no-tasks {
    opacity: 0.5;
  }
</style>