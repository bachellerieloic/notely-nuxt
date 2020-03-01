<template>
  <section class="section">
    <div class="columns is-mobile">
      <card
        title="Fetch"
        icon="github-circle"
      >
        <b-button @click="fetchTodos">
          Click Me
        </b-button>
        <b-button @click="todos = []">
          Reset
        </b-button>
      </card>
      <card title="Result" icon="">
        {{ todos }}
      </card>
    </div>
    <b-table :data="todos" :columns="columns" class="column is-full" >
      <template slot-scope="props">
        <b-table-column field="actions" label="Actions" width="40">
          {{ props.row.title }}
          <b-button @click="todos = []">
            Reset
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
      checkbox: false,
      checkboxCustom: 'Yes',
      todos: [],
      columns: [
        { field: 'id', label: 'ID', width: '20', numeric: true },
        { field: 'title', label: 'title', width: '40', numeric: true },
        { field: 'desc', label: 'description', width: '40', numeric: true },
        { field: 'isComplete', label: 'Completed', width: '40', numeric: true },
        { field: 'actions', label: 'Actions', width: '40', numeric: false }
      ]
    }
  },
  methods: {
    fetchTodos () {
      this.$axios.get('/todos')
        .then((response) => {
          this.todos = response.data
        })
    },
    delete(id) {
      this.$axios.delete('/todos', id)
        .then((response) => {
          console.log(response)
          this.todos = response.data
        });
    }
  }
}
</script>
