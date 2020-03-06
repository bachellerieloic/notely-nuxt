<template>
  <section class="section">
    <div class="columns is-mobile">
      <card
        title="Fetch"
        icon="github-circle"
      >
        <b-button @click="fetchTodos" :loading="loadingGetTodos">
          Get Todos
        </b-button>
        <b-button @click="todos = []">
          Reset
        </b-button>
      </card>
      <card title="Result" icon="">
        {{ todos }}
      </card>
    </div>
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
  </section>
</template>

<script>
import Card from '~/components/Card'

export default {
  name: 'HomePage',

  components: {
    Card
  },
  data () {
    return {
      loadingGetTodos: false,
      checkbox: false,
      checkboxCustom: 'Yes',
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
    deleteTodo (todoId) {
      this.$axios.delete('/todos', { data: { id: todoId } })
        .then((response) => {
          this.todos = response.data
        })
    },
    getIsComplete (isComplete) {
      return isComplete ? 'tag is-success' : 'tag is-danger'
    }
  }
}
</script>
