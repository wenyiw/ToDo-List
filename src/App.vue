<template>
  <div id="app">
    <h1> {{ title }} </h1> <!--<h1 v-text="title"></h1>-->
    <input v-model="newItem" @keyup.13="add">

    <!-- 
      the following ul euqals to:
      <div v-for="item in items" :class="{isFinished: item.finished}" 
                                @click="toggleFinished(item)">

        {{ item.label }}
      </div>
    -->
    <ul>
        <li v-for="(item, index) in items">
          <input type="checkbox" @click="toggleFinished(item)"></input>
          <p :class="['item', {isFinished: item.finished}]"> {{ item.label }} </p>
          <p class='delete' @click='del(index)'>delete</p>
        </li>
    </ul>
  </div>

  <!-- 
    below is just an example for components listening to each other
    it's technically not part of (or required for) the todo-list project
    <div>
      <!-
        child is the child component (if component is ChildA, here should be child-a)
        msg should be a variable in data in the child component
        'message from parent' is an example string; the child component will display this
        msg-from-child is defined in emit
      ->
      <child msg='message from parent' @msg-from-child='listenToChild'></child>

      <!-
        msgFromChild is in data (), contains what from @msg-from-child='listenToChild'
        listenToChild is in method which processes what's from child
      ->
      <p>{{ childMsg }}</p>

    </div>
  -->
</template>

<script>
import Store from './store'
import Child from './components/Child' //get the child component

export default {
  name: 'App',
  data () {
    return {
      title: 'ToDo List',
      items: Store.fetch(),
      newItem: '',
      childMsg: ''
    }
  },

  components: {Child}, //register the child component

  watch: {
    items: {
      handler: function(items) {
        Store.save(items)
      },
      deep: true
    }
  },

  methods: {
    toggleFinished: function(item) {
      item.finished = !item.finished
    },
    add: function() {
      this.items.push({
        label: this.newItem, 
        finished: false
      })
      this.newItem = ''
    },
    del: function (index) {
      this.items.splice(index, 1)
    },
    listenToChild: function(msg) { //Process the msg heard from child
      this.childMsg = msg
    }
  }
}
</script>


<style>
.isFinished {text-decoration: line-through;}
.item {display: inline; font-size: 1.5em;}
.delete {color: red; font-size: 0.8em; display: inline;}
.delete:hover {text-decoration: underline;}
ul {list-style-type: none; width: 200px; margin: 0px auto; text-align:left}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
