<template>
  <div>
    <h1>List</h1>
    <div class="row">
     <div class="col s6"> 
      <select ref="select" v-model="filter">
        <option value="" disabled selected>Choose your option</option>
        <option value="active">Active</option>
        <option value="outdate">Outdate</option>
        <option value="completed">Completed</option>
    </select>
    <label>Status filter</label>
    <button v-if="filter" class="btn btn-small red" type="button" @click="filter = null">Clear filter</button>
    </div>

    </div>
    <hr>
    <table v-if="tasks.lenght = true">
      <thead>
        <tr>
          <th>#</th>
          <th>Title</th>
          <th>Date</th>
          <th>Description</th>
          <th>Status</th>
          <th>Open</th>
        </tr>
      </thead>
      <tbody>
        <tr 
        v-for="(task, idx) of displayTasks"
        :key="task.id"
        >
        <td>{{idx + 1}}</td>
        <td>{{task.title}}</td>
        <td>{{new Date(task.date).toLocaleDateString()}}</td>
        <td class="td-width"><div class="text-description">{{task.description}}</div></td>
        <td>{{task.status}}</td>
        <td>
          <router-link tag="button" class="btn btn-small" :to="'/task/' + task.id">
            Open
          </router-link>
        </td>
        </tr>
      </tbody>
      <button class="btn btn-small red" @click="clearTasks()" style="margin-top: 1rem">detele all tasks</button>
    </table>
    <p v-else>No tasks</p>
  </div>
</template>

<script>
export default {
  data: () => ({
    filter: null,

  }),
  computed: {
    tasks() {
      return this.$store.getters.tasks
    },
    displayTasks() {
      return this.tasks.filter(t => {
        if(!this.filter) {
          return true
        }
        return t.status === this.filter
      })
    }
  },
  mounted() {
    M.FormSelect.init(this.$refs.select)
  },
  methods: {
    clearTasks(){
      localStorage.clear()
      document.location.reload();
    }
  }
}

</script>

<style lang="scss" scoped>
  .td-width{
    max-width: 400px;
  }
  .text-description{
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
  }
</style>