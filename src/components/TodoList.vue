<template>
  <div class="todolist-v1">
    <div class="container">
      <div class="row">
        <div class="col-12">
          <h2>{{inputText}}</h2>
          <input v-model="newItem" type="text"/>
          <button @click.self="addToList">Add</button>


          <ul>
            <li v-for="listItem in listItems">
              {{listItem.text}}
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
    }
  }
}
</script>