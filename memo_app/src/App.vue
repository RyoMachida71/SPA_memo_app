<template>
  <div id="app">
    <h1>メモアプリ</h1>
      <form @submit.prevent>
        <textarea v-model="newMemo" v-show="newForm" cols="40" rows="20"></textarea>
        <button  @click="add_Memo()" v-show="newForm" >作成</button>
        <button @click="createNewMemo" v-show="createButton">新規</button>
      </form>
    <ul>
      <li v-for="(memo, index) in memos" :key='index'>
        <a href='#' @click="showDetails(memo)">{{ memo.title }}</a>
      </li>
    </ul>
    <textarea v-model="editMemo" v-show="editForm" cols="40" rows="20"></textarea>
    <button @click="edit_Memo(memo)" v-show="editForm">変更</button>
    <button @click="delete_Memo(index)" v-show="editForm" >削除</button>
        {{ $data }}
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
      var str1 = this.newMemo
      var str2 = this.newMemo.split("\n");
      this.memos.push({
        memo: str1,
        title: str2[0],
      });
      this.newMemo = '';
      this.newForm = false;
      this.createButton = true;
      this.saveMemo();
    },
    showDetails: function(memo){
      this.editForm = true;
      this.editMemo = memo.memo
    },
    edit_Memo: function(memo){
        const targetIndex = this.memos.indexOf(memo);
        var str1 = this.editMemo;
        var str2 = this.editMemo.split("\n");
        this.memos.splice(targetIndex, 1, {
          memo: str1,
          title: str2[0],
        }) 
        this.editMemo = '';
        this.editForm = false;
        this.saveMemo();
    },
    delete_Memo: function(index){
        this.memos.splice(index, 1);
        this.editForm = false;
        this.saveMemo();
      }
  }
}
</script>

<style>

</style>
