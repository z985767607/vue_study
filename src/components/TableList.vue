<template>git
  <div id="demo">
    <div class="chectoutid">
      <input placeholder="id查询" style="width:200px;height:20px;" id="searchid" v-model="search"/>
      <button style="width:50px;height:20px;" @click="filterIds()">button</button>
    </div>
<br>
    <div>
      <input placeholder="分类查询" style="width:200px;height:20px;">
      <button style="width:50px;height:20px;">分类</button>
    </div>
    <div>
      <select v-model="selected">
        <option value="">请选择</option>
        <option >json</option>
        <option >server</option>
        <option >json-server</option>
      </select>
      <button v-on:click="data()">selected:{{selected}}</button>
    </div>
    <div>
      <table id="table1">
        <caption>表格数据</caption>
        <thead>
        <tr>
          <th>id</th>
          <th>分类</th>
          <th>author</th>
        </tr>
        </thead>
        <tbody>
          <tr v-for="item in items" class="trs">
            <td class="tds" id="col0" >{{item.id}}</td>
            <td class="tds" v-html="item.classify">{{item.classify}}</td>
            <td class="tds" v-html="item.author" @click="clickdel(item.id)">{{item.author}}</td>
          </tr>
        </tbody>
      </table>

    </div>
  </div>
</template>

<script>
  import axios from 'axios'

    export default {
      name: "TableList",

      data(){
        return {
          tables:[],
          selected:[],
          search: ''
        }
      },
  /*    data() {
        return {
          tables: [],
          selected:''
        }
      },
      */
      mounted() {
        debugger
        this.fetchTables()
      },
      methods: {
        fetchTables: function () {
          fetch('http://localhost:8080/api/face')
            .then(response => response.json())
            .then(json => {
            //this.tables = json[0]
            this.tables = json;
            console.log("我请求的数据："+json)
        })
        },
        clickdel(id) {
          console.log("-------")
          debugger
          fetch('http://localhost:8080/api/face', {
            method: 'POST',
            body:JSON.stringify({
              id: id
            }) // 这里是请求对象
          }).then(response => response.json())
            .then(json => {
              //this.tables = json[0]
              this.tables = json;
              console.log("我请求的数据："+json)
            })
        }
      },
      computed: {
/*
        filterIds: function () {
            var searchContent = document.getElementById("searchid").value;
            var storeId = document.getElementById('table1');//获取table的id标识
            var rowsLength = storeId.rows.length;//表格总共有多少行
            for(var i=1;i<rowsLength;i++){//按表的行数进行循环，本例第一行是标题，所以i=1，从第二行开始筛选（从0数起）
              var searchText = storeId.rows[i].cells[0].innerHTML;//取得table行，列的值
              if(searchText.match(searchContent)){//用match函数进行筛选，如果input的值，即变量 key的值为空，返回的是ture，
                storeId.rows[i].style.display='';//显示行操作，
              }else{
                storeId.rows[i].style.display='none';//隐藏行操作
              }
            }
        },
        items: function() {
          var _search = this.search;
          if (_search) {
            return this.products.filter(function(product) {
              return Object.keys(product).some(function(key) {
                return String(product[key]).toLowerCase().indexOf(_search) > -1
              })
            })
          }

          return this.products;
        },
        */
        items:function(){
          var _search = this.search;
          if(_search){
            return this.tables.filter(function(table){
              return Object.keys(table).some(function(key){
                return String(table[key]).toLowerCase().indexOf(_search)>-1
              })
            })
          }
          return this.tables;
        }
      }

    }
</script>

<style scoped>

.tds{
  border:1px solid black
}
</style>
