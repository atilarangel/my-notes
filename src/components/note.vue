<template>
  <div class="note">
    <a href="#"><div v-on:click="changeLang()" style="position: inherit; width: 30px; margin: -35px -10px -15px 0px;float: right; font-size: 20px; font-family: 'Open Sans', sans-serif;color: #06D85F; border-style: solid; border-width: 2px;">{{ lang }}</div></a>
    <h1>{{ title }}</h1>
    <input type="text" v-model='text' v-bind:placeholder="list_alert2" v-on:keyup.enter='addAll()'>
    <p class = 'alert' v-if='exist'>{{ list_alert1 }}</p>
    <p class = 'alert' v-if='empty'>{{ list_alert2 }}</p>
    <a href="#" v-on:click='addAll()'><img src="../assets/add_button.png"></a>
    <div>
    <draggable class="warp-div" v-model="list" :options="{group:'people'}" @start="drag=true" @end="drag=false">
      <div class="do" :style="'background-color:'+element.color" v-for="element in list"  v-bind:key="element.text">
        <a href="#" v-on:click="element.show = !element.show">{{ element.text }}</a>
      <p><textarea v-show="element.show" placeholder="Add a description here"></textarea></p>
      </div>
    </draggable>
    </div>
    <br>
    <br>
    <div>
    <draggable class="warp-div" v-model="listdone" :options="{group:'people'}" @start="drag=true" @end="drag=false">
      <transition-group tag="div" mode="out-in" name="disabled">
        <div class="done" :style="'background-color:'+element.color" v-for="element in listdone" v-bind:key="element.text"><p v-on:click="element.show = !element.show">{{ element.text }}</p>
        <p><textarea v-show="element.show" placeholder="Add a description here"></textarea></p></div>
        </transition-group>
    </draggable>
    </div>
    <div class="box-trash">
    <draggable class="warp-div" v-model="listTrash" :options="{group:'people'}" @start="drag=true" @end="drag=false">
      <img class='trash_button'>
      <div class="trash" :style="'background-color:'+element.color" v-for="element in listTrash" v-bind:key="element.text"></div>
    </draggable>
    </div>
    <!--<ul class="do">
      <li class="title">Do</li>
      <draggable v-model="list">
      <li v-on:click='add()' v-for="item in list" :key="item.li">{{ item.text }}</li>
      </draggable>
    </ul>
    <ul class="done">
      <li>Done</li>
      <draggable v-model="listdone">
      <li  v-on:click='add()' v-for="item in listdone" :key="item.li">{{ item.text }}</li>
      </draggable>
    </ul>
    <div class="box-text"></div>-->
  </div>
</template>
<script>
import draggable from 'vuedraggable'
export default {
  components: {
    draggable
  },
  data () {
    return {
      list: [{text: 'Do', subt: 'Clickaaaaaaaaaaaaaaaaaaaaa', show: false, order: 1}],
      listdone: [{text: 'Done', subt: 'Clickaaaaaaaaaaaaaaaaaaaaa', show: false, order: 1}],
      text: '',
      listTrash: [{text: 'Trash', order: 1}],
      exist: false,
      empty: false,
      lang: 'Pt',
      title: 'Task List',
      list_alert1: 'This task has been in list',
      list_alert2: 'Write some task'
    }
  },
  methods: {
    addAll () {
      if (this.text === '' || this.text === ' ') {
        console.log('Write some task')
        this.empty = true
      } else if (this.text in this.list) {
        console.log('This task has been in list')
        this.exist = true
      } else {
        this.list.push({text: this.text, subt: 'Add description', show: false, order: 0})
        this.empty = false
        this.exist = false
        this.text = ''
      }
    },
    changeLang () {
      if (this.lang === 'Pt') {
        this.lang = 'En'
        this.title = 'Lista de Tarefas'
        this.list_alert1 = 'Esta tarefa já está na lista'
        this.list_alert2 = 'Escreva alguma tarefa'
      } else {
        this.lang = 'Pt'
        this.title = 'Task List'
        this.list_alert1 = 'This task has been in list'
        this.list_alert2 = 'Write some task'
      }
    }
  }
}
</script>

<style scoped>
h1 {
  margin-top: 60px;
}
h1, h2, h3 {
  font-weight: normal;
  color: #06D85F;
}
img{
  padding:20px 20px 20px 20px;
}
a{
  text-decoration: none;
}
input{
  border: none;
  height: 30px;
  width: 100%;
  text-align: center;
  background-color: #333;
  border-bottom: 3px solid #06D85F;
  color: white;
  font-size: 20px;
  outline: none;
}
textarea{
  width: 90%;
  background-color: transparent;
  border: none;
  color: #333;
  outline: none;
}

.box-trash{
  position: fixed;
  width: 100%;
  left: 0;
  bottom: 0;
  background-image: url(../assets/trash_button.png);
  background-repeat: no-repeat;
  background-position: center top;
  box-shadow: 1px 2px 5px black;
  background-color: #06D85F;
}
.box-trash:hover{
  background-image: url(../assets/add_button2.png);
  background-repeat: no-repeat;
  background-position: center top;
}
.alert{
  font-size: 15px;
  color: #ff4646;
}
.do {
  word-break: break-all;
  text-align: left;
  margin: 10px 0px 0px -8px;
  width: 100%;
  padding: 20px 10px 20px 10px;
  background-color: #06D85F;
  color: white;
}
.do a{
  color: white;
}
.done {
  word-break: break-all;
  text-align: left;
  margin: 10px 0px 0px -8px;
  width: 100%;
  padding: 20px 10px 20px 10px;
  background-color: #3f3f3f;
  color: white;
}
.disabled-enter-active, .disabled-leave-active {
  transition: none !Important;
}
</style>
