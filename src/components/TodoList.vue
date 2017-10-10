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
              <input type="checkbox" v-model="listItem.checked">{{listItem.text}}<button @click.self="deleteItem(listItem)">Delete</button></div>
          </draggable>
        </div>
      </div>
    </div>
    <!-- <pre>{{$data}}</pre> -->
  </div>

</template>

<script>
import draggable from 'vuedraggable'
export default {
  name: 'TodoList',
  components: {
    draggable
  },
  data () {
    return {
      inputText: 'bingo bongo',
      newItem: '',
      listItems: []
    }
  },
  methods: {
    addToList: function () {
      var item = this.newItem.trim()
      if (item) {
        this.listItems.push(
          {
            text: item,
            checked: false
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
    }
  }
}
</script>