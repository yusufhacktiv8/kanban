<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-md-3">
        <div class="panel panel-default">
          <div class="panel-heading">
            <h3 class="panel-title">
              Back Log
              <b-form-input v-model="taskDescription"
                  type="text"
                  placeholder="Task"
              ></b-form-input>
              <b-button variant="warning" href="" @click="addTask">
                Add
              </b-button>
            </h3>
          </div>
          <div class="panel-body">
            <b-card class="mb-2" variant="" v-for="task in backLogs">
              {{ task.description }}
              <div class="">
                <b-button variant="danger" href="" @click="removeTask(task)">
                  Remove
                </b-button>
                <b-button variant="success" href="" @click="nextTask(task)">
                  Next
                </b-button>
              </div>
            </b-card>
          </div>
        </div>
      </div>
      <div class="col-md-3">
        <div class="panel panel-default">
          <div class="panel-heading">
            <h3 class="panel-title">Todo</h3>
          </div>
          <div class="panel-body">
            <b-card class="mb-2" variant="" v-for="task in todos">
              {{ task.description }}
              <div class="">
                <b-button variant="info" href="" @click="backTask(task)">
                  Back
                </b-button>
                <b-button variant="success" href="" @click="nextTask(task)">
                  Next
                </b-button>
              </div>
            </b-card>
          </div>
        </div>
      </div>
      <div class="col-md-3">
        <div class="panel panel-default">
          <div class="panel-heading">
            <h3 class="panel-title">Doing</h3>
          </div>
          <div class="panel-body">
            <b-card class="mb-2" variant="" v-for="task in doings">
              {{ task.description }}
              <div class="">
                <b-button variant="info" href="" @click="backTask(task)">
                  Back
                </b-button>
                <b-button variant="success" href="" @click="nextTask(task)">
                  Next
                </b-button>
              </div>
            </b-card>
          </div>
        </div>
      </div>
      <div class="col-md-3">
        <div class="panel panel-default">
          <div class="panel-heading">
            <h3 class="panel-title">Done</h3>
          </div>
          <div class="panel-body">
            <b-card class="mb-2" variant="" v-for="task in dones">
              {{ task.description }}
              <div class="">
                <b-button variant="info" href="" @click="backTask(task)">
                  Back
                </b-button>
                <b-button variant="danger" href="" @click="removeTask(task)">
                  Remove
                </b-button>
              </div>
            </b-card>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: 'hello',
  data() {
    return {
      taskDescription: '',
    };
  },
  methods: {
    addTask() {
      this.$db.ref('mykanban').push({
        description: this.taskDescription,
        status: 0,
      });

      this.taskDescription = '';
    },
    removeTask(task) {
      this.$db.ref('mykanban').child(task['.key']).remove();
    },
    nextTask(task) {
      const theTask = task;
      theTask.status += 1;
    },
    backTask(task) {
      const theTask = task;
      theTask.status -= 1;
    },
  },
  computed: {
    backLogs() {
      return this.tasks.filter(task => task.status === 0);
    },
    todos() {
      return this.tasks.filter(task => task.status === 1);
    },
    doings() {
      return this.tasks.filter(task => task.status === 2);
    },
    dones() {
      return this.tasks.filter(task => task.status === 3);
    },
  },
  firebase() {
    return {
      tasks: this.$db.ref('mykanban'),
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
