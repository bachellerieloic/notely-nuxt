<template>
  <section class="section">
    <div class="columns">
      <div class="column is-one-quarter">
        <div class="card">
          <header class="card-header">
            <p class="card-header-title">
              Todo
            </p>
          </header>
          <div class="card-content">
            <div class="content">
              <b-field label="Title">
                <b-input v-model="todoForm.title" />
              </b-field>
              <b-field label="Description">
                <b-input v-model="todoForm.desc" />
              </b-field>
              <b-field>
                <b-button :loading="loadingSaveTodo" @click="saveTodo()">
                  Save Todo
                </b-button>
                <b-button :loading="loadingGetTodos" @click="fetchTodos">
                  Get Todos
                </b-button>
                <b-button @click="todos = []">
                  Reset
                </b-button>
              </b-field>
            </div>
          </div>
        </div>
      </div>
      <div class="column">
        <div class="card">
          <header class="card-header">
            <p class="card-header-title">
              Result
            </p>
          </header>
          <div class="card-content">
            <div class="content">
              <b-table :data="todos" :columns="columns" class="column is-full">
                <template slot-scope="props">
                  <b-table-column field="title" label="title" centered>
                    <span>{{ props.row.title }}</span>
                  </b-table-column>
                  <b-table-column field="desc" label="Description" centered>
                    <span>{{ props.row.desc }}</span>
                  </b-table-column>
                  <b-table-column field="isComplete" label="Completed" centered>
                    <span :class="getIsComplete(props.row.isComplete)">
                      {{ props.row.isComplete }}
                    </span>
                  </b-table-column>
                  <b-table-column field="action" label="Action" centered>
                    <b-button class="tag is-success" @click="deleteTodo(props.row.id)">
                      Delete
                    </b-button>
                  </b-table-column>
                </template>
              </b-table>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>

export default {
  name: 'HomePage',

  components: {
  },
  data () {
    return {
      loadingSaveTodo: false,
      loadingGetTodos: false,
      checkbox: false,
      checkboxCustom: 'Yes',
      todoForm: {
        title: '',
        desc: '',
        isComplete: false
      },
      todos: [],
      columns: [
      ]
    }
  },
  methods: {
    fetchTodos () {
      this.loadingGetTodos = true
      this.$axios.get('/todos')
        .then((response) => {
          this.loadingGetTodos = false
          this.todos = response.data
        })
    },
    saveTodo () {
      if (!this.validate()) {
        return false
      }
      this.loadingSaveTodo = true
      this.$axios.post('/todos', this.todoForm)
        .then((response) => {
          this.loadingSaveTodo = false
          this.fetchTodos()
        })
    },
    deleteTodo (todoId) {
      this.$axios.delete('/todos/' + todoId)
        .then((response) => {
          this.fetchTodos()
        })
    },
    getIsComplete (isComplete) {
      return isComplete ? 'tag is-success' : 'tag is-danger'
    },
    validate () {
      return !(this.todoForm.title.length === 0 && this.todoForm.desc.length === 0)
    }
  }
}
</script>
