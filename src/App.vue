<template>
  <div id="app">
    <div class="header clearfix">
      <div class="date">
        <p class="day">{{ day | date }}</p>
        <div class="more">
          <p class="mouth">{{ month | date }}</p>
          <p class="year">{{ year }}</p>
        </div>
      </div>
      <div class="time">{{ H }} : {{ M }}</div>
    </div>
      <transition name="old"  appear>
        <todo-list :items="ListItems" v-show="fade" @delItem="delItem" @impItem="impItem" @editItem="editItem" ></todo-list>
      </transition>
    <div class="add">
      <mu-button fab color="#555" @click="fade = !fade">
        <mu-icon value="add"></mu-icon>
      </mu-button>
    </div>
    <transition name="new" mode="out-in">
      <create-item v-if="!fade" @sub="addItem"></create-item>
    </transition>
    <edit-item v-if="edit" :item="ListItems[editid]" :index="editid" @applyEdit="apply"></edit-item>
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue'
import CreateItem from './components/CreateItem.vue'
import EditItem from './components/EditItem.vue'

export default {
  name: 'app',
  data(){
    return {
      day: new Date().getDate(),
      month: new Date().getMonth(),
      year: new Date().getFullYear(),
      H: new Date().getHours(),
      M: new Date().getMinutes(),
      fade: true,
      edit: false,
      editid: null,
      ListItems: JSON.parse(window.localStorage.getItem('List')) || []
    }
  },
  created(){
    this.timeFormate()
  },
  methods: {
    timeFormate() {
      setInterval(() => {
        this.H = new Date().getHours() < 10 ? '0' + new Date().getHours() : new Date().getHours();
        this.M = new Date().getMinutes() < 10 ? '0' + new Date().getMinutes() : new Date().getMinutes();
      }, 1000);
	},
    addItem(item){
        this.ListItems.push(item)
        this.fade = true
	},
    delItem(id){
        this.ListItems.splice(id, 1)
    },
    impItem(id){
		console.log(this.ListItems[id].important);
		this.ListItems[id].important = !this.ListItems[id].important
    },
    editItem(id){
        this.editid = id
    	this.edit = true
	},
    apply(index, data) {
    	this.ListItems.splice(index, 1)
        this.ListItems.splice(index, 0, data)
		this.edit = false
    }
  },
  watch: {
    ListItems: {
        deep: true,
        handler: function (newlist, oldlist) {
            window.localStorage.setItem('List', JSON.stringify(newlist))
        }
    }
  },
  filters: {
    date: function (val) {
      if (parseInt(val) < 10) {
        return '0' + val
      }
    }
  },
  components: {
  	TodoList,
    CreateItem,
    EditItem
  }
}
</script>

<style scoped>
  .edit-item {
    position: absolute;
    left: 100%;
  }
  .old-enter-active, .old-leave-active {
    transition: all .4s ease;
  }
  .old-enter, .old-leave-to {
    opacity: 0;
    transform: translateX(-50%);
  }
  .new-enter-active, .new-leave-active {
    transition: all .5s ease;
  }
  .new-enter, .new-leave-to {
    opacity: 0;
    transform: translateX(50%);
  }
  .header {
    width: 100%;
    padding: 20px;
  }
  .date {
    float: left;
    display: flex;
    align-items: center;
  }
  .date > p {
    font-size: 40px;
    line-height: 40px;
    float: left;
  }
  .date > .more {
    color: #999;
    line-height: 18px;
    margin-left: 10px;
    float: right;
  }
  .time {
    font-size: 36px;
    line-height: 40px;
    display: flex;
    align-items: center;
    float: right;
  }
  #app > div.add {
    width: 100%;
    text-align: center;
    position: absolute;
    bottom: 30px;
  }
</style>
