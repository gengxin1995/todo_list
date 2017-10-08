<template>
  <div id="app">
    <h1>{{title}}</h1>
    <input v-model="newItem" v-on:keyup.enter="addNew"/>
    <ul>
      <li v-for="item in items" v-bind:class="{finished: item.isFinished}" v-on:click="toggleFinish(item)">
        {{item.lable}}
      </li>
    </ul>
  </div>
</template>

<script>
  import Storage from './localstorage'
  export default {
    name: 'app',
    data () {
      return {
        title: 'this is a todo list',
        items: Storage.fetch(),
        newItem: ''
      }
    },
    methods: {
      toggleFinish: function (item) {
        item.isFinished = !item.isFinished
      },
      addNew: function () {
        this.items.push({
          lable: this.newItem,
          isFinished: false
        })
        this.newItem = null
      }
    },
    watch: {
      items: {
        handler: function (items) {
          Storage.save(items)
        },
        deep: true
      }
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  .finished{
    color: red;
  }
</style>
