<!-- 
  Example of transferring info between components
  This is the child component
  The parent component is App.vue
-->

<template>
  <div>
    <h1>{{ msg }}</h1> <!-- this is from App.vue (the parent component) -->
    <button @click='speakToParent'> speak to parent </button>
  </div>
</template>

<script>
export default {
  name: 'Child',
  data () {
    return {
    }
  },

  props: ['msg'], //variable listen to other components go here (props instead of data)

  methods: {
    speakToParent: function() {
      //from <child msg='message from parent' @msg-from-child='listenToChild'></child>
      this.$emit('msg-from-child', 'message from child') 
    }
  }

}
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
