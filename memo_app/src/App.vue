<template>
  <div id="app">
    <h1>メモアプリ</h1>
      <form @submit.prevent class="newForm">
        <textarea v-model="newMemo" v-show="newForm" cols="40" rows="35"></textarea> <br>
        <button  @click="add_Memo()" v-show="newForm" class="createButton">作成</button>
      </form>
    <ul>
      <li v-for="(memo, index) in memos" :key='index'>
        <a href='#' @click="showDetails(memo)" class="memo">{{ memo.title }}</a>
      </li>
    </ul>
    <a href="#" @click="createNewMemo" v-show="createButton" class="addButton">＋</a>
    <form @submit.prevent class="editForm">
      <textarea v-model="editMemo" v-show="editForm" cols="40" rows="35"></textarea>
      <button @click="edit_Memo(memo)" v-show="editForm" class="editButton">変更</button>
      <button @click="delete_Memo(memo)" v-show="editForm" class="deleteButton" >削除</button>
    </form>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return{
      newMemo: '',
      memos: [],
      editMemo: '',
      createButton: true,
      newForm: false,
      editForm: false,
    }
  },
  mounted(){
    this.memos = JSON.parse(localStorage.getItem('memos')) || [];
  },
  methods:{
    saveMemo: function(){
        localStorage.setItem('memos', JSON.stringify(this.memos))
      },
    createNewMemo: function(){
      this.newForm = true;
      this.createButton = false;
    },
    add_Memo: function(){
      var newMemo = this.newMemo
      var title = this.newMemo.split("\n");
      this.memos.push({
        memo: newMemo,
        title: title[0],
      });
      this.newMemo = '';
      this.newForm = false;
      this.createButton = true;
      this.saveMemo();
    },
    showDetails: function(memo){
      if(this.newForm == true){
        this.editForm = false;
      }else{
      this.editForm = true;
      }
      this.editMemo = memo.memo
    },
    edit_Memo: function(memo){
        const targetIndex = this.memos.indexOf(memo);
        var editMemo = this.editMemo;
        var title = this.editMemo.split("\n");
        this.memos.splice(targetIndex, 1, {
          memo: editMemo,
          title: title[0],
        }) 
        this.editMemo = '';
        this.editForm = false;
        this.saveMemo();
    },
    delete_Memo: function(memo){
      const targetIndex = this.memos.indexOf(memo)
        this.memos.splice(targetIndex, 1);
        this.editForm = false;
        this.saveMemo();
      }
  }
}
</script>

<style>
#app {
  width: 600px;
  height: 600px;
  margin: auto;
  border: 2px solid brown; 
  position: relative;
}
#app h1{
  background-color: brown;
  color: white;
  text-align: center;
  margin-top: 0;
}
#app .addButton{
  font-size: 25px;
  padding: 5px 5px 5px 15px;
}
#app .createButton{
  width: 300px;
  height: 30px;
}
#app .newForm{
  position: absolute;
  top: 70px;
  left: 280px;
}
#app .memo{
  font-size: 20px;
}
#app .editForm{
  position: absolute;
  top: 70px;
  left: 280px;
}
#app .editButton{
  width: 200px;
  margin-right: 5px;
  height: 30px;
}
#app .deleteButton{
  width: 100px;
  height: 30px;
}
</style>
