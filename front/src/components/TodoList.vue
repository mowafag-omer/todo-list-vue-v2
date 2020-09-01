<template>
    <ul class="b-list-group">
      <singleTodo v-for="item in tdlist" :key="item.name" @update="getData" :item='item'/>
    </ul> 
</template>

<script>
import SingleTodo from './SingleTodo'
import axios from 'axios'

export default {
  name: 'todoList',
  components: {
    SingleTodo
  },
  props: {
    tdlist: Array,
    whatToDisplay: String
  },
  methods:{
    getData: async function() {
      try{ 
        const api = await axios.get('http://localhost:3030/todo')
        const list = await api
        
        this.whatToDisplay == 'all' && (this.tdlist = list.data)
        this.whatToDisplay == 'todo' && (this.tdlist = list.data.filter(elm => elm.todo == true && elm))
        this.whatToDisplay == 'done' && (this.tdlist = list.data.filter(elm => elm.todo == false && elm))
      } 
      catch(err) { console.error(err) }
    }
  },
  mounted: function() {
    this.getData()
  } 
}
</script>

<style scoped>
li {
  list-style: none;
}
</style>