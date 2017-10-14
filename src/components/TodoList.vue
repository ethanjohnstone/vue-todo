<template>
  <div class="todolist-v1">
    <div class="container">
      <div class="row">
        <div class="col-12">
          <h2>{{inputText}}</h2>
          <input v-model="newItem" v-on:keyup.enter="addToList" type="text"/>
          <button  @click.self="addToList">Add</button>
          <button @click.self="voidList">Void List</button>
          <button @click.self="clearSelected">Clear Selected</button>
          <draggable class="todo-list">
            <div class="list-items"v-for="listItem in listItems">
              <input type="checkbox" v-model="listItem.checked">
              <label @dblclick.self="editItem(listItem)" v-if="!listItem.clicked">{{listItem.text}}</label>
              <input v-model="listItem.text" type="text" v-on:keyup.enter="doneEditing(listItem)" v-on:keyup.esc="cancelEdit(listItem)" v-bind:value="listItem.text" v-if="listItem.clicked">
              <button @click.self="deleteItem(listItem)">Delete</button>
            </div>
          </draggable>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import draggable from 'vuedraggable'

var STORAGE_KEY = 'BingoBongo'
var todoStorage = {
  fetch: function () {
    var thingsTodo = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]')
    thingsTodo.forEach(function (todo, index) {
      todo.id = index
    })
    todoStorage.uid = thingsTodo.length
    return thingsTodo
  },
  save: function (todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos))
  }
}
export default {
  name: 'TodoList',
  components: {
    draggable
  },
  data () {
    return {
      inputText: 'bingo bongo',
      newItem: '',
      listItems: todoStorage.fetch()
    }
  },
  watch: {
    listItems: {
      handler: function (todos) {
        todoStorage.save(todos)
      },
      deep: true
    }
  },
  methods: {
    addToList: function () {
      var item = this.newItem.trim()
      if (item) {
        this.listItems.push(
          {
            text: item,
            checked: false,
            clicked: false,
            tempText: ''
          }
        )
        this.newItem = ''
      }
    },
    voidList: function () {
      this.listItems = []
    },
    deleteItem: function (item) {
      var indexOfItem = this.listItems.indexOf(item)
      this.listItems.splice(indexOfItem, 1)
    },
    clearSelected: function () {
      // TODO: Replace for a foreach version?
      for (var i = this.listItems.length - 1; i >= 0; i--) {
        if (this.listItems[i].checked) {
          this.deleteItem(this.listItems[i])
        }
      }
    },
    editItem: function (listItem) {
      listItem.clicked = !listItem.clicked
      listItem.tempText = listItem.text
    },
    doneEditing: function (listItem) {
      listItem.clicked = false
      listItem.text = listItem.text.trim()
    },
    cancelEdit: function (listItem) {
      listItem.clicked = false
      listItem.text = listItem.tempText
    }
  }
}
</script>