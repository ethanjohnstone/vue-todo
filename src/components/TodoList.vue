<template>
  <div class="todolist-v1">
    <div class="container">
      <div class="row">
        <div class="col-12">
          <h2>{{inputText}}</h2>
          <input v-model="newItem" v-on:keyup.enter="addToList" type="text"/>
          <button  @click.self="addToList">Add</button>
          <button @click.self="voidList">Void List</button>
          <ul>
            <li v-for="listItem in listItems">
              {{listItem.text}}<button @click.self="deleteItem(listItem)">Delete</button>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <!-- <pre>{{$data}}</pre> -->
  </div>

</template>

<script>
	
export default {
  name: 'TodoList',
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
            text: item
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
    }
  }
}
</script>