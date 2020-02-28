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
      todos: []
    }
  },
  methods: {
    fetchTodos () {
      this.$axios.get('/todos')
        .then((response) => {
          console.log(response)
          this.todos = Parse.esponse.data
        })
    }
  }
}
</script>
