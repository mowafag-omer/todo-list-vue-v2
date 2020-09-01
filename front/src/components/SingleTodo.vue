<template>
  <li v-if="item.todo == false" class="done"><b-icon-check-circle-fill/><span @click="toggle"> {{ item.name }} </span><b-icon-trash class="ml-1" @click="deleteTodo" /></li> 
  <li v-else><b-icon-check2-circle/><span @click="toggle"> {{ item.name }} </span><b-icon-trash class="ml-1" @click="deleteTodo" /></li> 
</template>

<script>
import axios from 'axios'

export default {
  name: 'SingleTodo',
  props: ['item'],
  methods: {
    toggle: async function() {
      try{ 
        await axios.put('http://localhost:3030/todo', {
          id: this.item.id,
          todo: this.item.todo
        })
        this.$emit('update')
      } 
      catch(err) { console.error(err) }
    },
    deleteTodo: async function() {
      console.log('single');
      console.log(this.item.id);
      try{ 
        await axios.delete(`http://localhost:3030/todo/${this.item.id}`)
        this.$emit('update')
      } 
      catch(err) { console.error(err) }
    }  
  }  
}
</script>

<style scoped>
span{
  cursor: pointer;
}
.done{
  text-decoration: line-through;
  color: gray;
}
</style>