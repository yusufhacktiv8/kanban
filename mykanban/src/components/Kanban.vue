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
            <b-card class="mb-2" variant="" v-for="task in tasks" v-if="task.status === 0">
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
            <b-card class="mb-2" variant="" v-for="task in tasks" v-if="task.status === 1">
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
            <b-card class="mb-2" variant="" v-for="task in tasks" v-if="task.status === 2">
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
            <b-card class="mb-2" variant="" v-for="task in tasks" v-if="task.status === 3">
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
      const theTask = { ...task };
      theTask.status += 1;
      delete theTask['.key'];
      this.$db.ref('mykanban').child(task['.key']).set(theTask);
    },
    backTask(task) {
      const theTask = { ...task };
      theTask.status -= 1;
      delete theTask['.key'];
      this.$db.ref('mykanban').child(task['.key']).set(theTask);
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
