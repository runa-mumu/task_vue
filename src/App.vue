<template>
  <div id="app">
    <h1>Todo List</h1>
    <div class = "card">
      <div class = "new">
        <input type="text" id="task" v-model="newTask" />
      </div>
      <button @click="insertTask">追加</button>
      <div class="update">
        <table>
          <tr>
            <th>ID</th>
            <th>Task</th>
          </tr>
          <tr v-for="item in taskLists" :key="item.id">
            <td>{{item.id}}</td>
            <td><input type="text" v-model="item.task" /></td>
            <td>
              <button @click="updateTask(item.id, item.task)">更新</button>
            </td>
            <td>
              <button @click="deleteTask(item.id)">削除</button>
            </td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return{
      newTask: "",
      taskLists: [],

    };
  },
  methods: {
    async getTask() {
      const resData = await axios.get("http://127.0.0.1:8000/api/task/");
      this.taskLists = resData.data.data;
    },
    async insertTask() {
      const sendData = {
        name: this.newTask,
      };
      await axios.post("http://127.0.0.1:8000/api/task/", sendData);
      await this.getTask();
    },
    async updateTask(id, task) {
      const sendData = {
        name: task,
      };
      await axios.put("http://127.0.0.1:8000/api/task/" + id, sendData);
      await this.getTask();
    },
    async deleteTask(id) {
      await axios.delete("http://127.0.0.1:8000/api/task/" + id);
      await this.getTask();
    },
  },
  created() {
    this.getTask();
  },
};
</script>

<style>
html, body, div, span, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
abbr, address, cite, code,
del, dfn, em, img, ins, kbd, q, samp,
small, strong, sub, sup, var,
b, i,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, figcaption, figure,
footer, header, hgroup, menu, nav, section, summary,
time, mark, audio, video {
    margin:0;
    padding:0;
    border:0;
    outline:0;
    font-size:100%;
    vertical-align:baseline;
    background:transparent;
}

body {
    line-height:1;
}

article,aside,details,figcaption,figure,
footer,header,hgroup,menu,nav,section {
    display:block;
}

nav ul {
    list-style:none;
}

blockquote, q {
    quotes:none;
}

blockquote:before, blockquote:after,
q:before, q:after {
    content:'';
    content:none;
}

a {
    margin:0;
    padding:0;
    font-size:100%;
    vertical-align:baseline;
    background:transparent;
}

/* change colours to suit your needs */
ins {
    background-color:#ff9;
    color:#000;
    text-decoration:none;
}

/* change colours to suit your needs */
mark {
    background-color:#ff9;
    color:#000;
    font-style:italic;
    font-weight:bold;
}

del {
    text-decoration: line-through;
}

abbr[title], dfn[title] {
    border-bottom:1px dotted;
    cursor:help;
}

table {
    border-collapse:collapse;
    border-spacing:0;
}

/*設定*/

h1{
  font-size: 30px;
}

input, select {
    vertical-align:middle;
}

html {
  background-color: #270ca1;
}
* {
  color: black;
  font-family: "Noto Sans JP";
}
.new{
  display: inline-block;
}

table,
td,
th {
  border: 1px solid #000;
  border-collapse: collapse;
  text-align: center;
  color: black;
}
td,
th {
  padding: 5px;
}
th {
  background: #f0e6cc;
}
button{
  color: #000;
  
}

#app{
  margin: 20px;
  width: 400px;
  background: #fff;
  border-radius: 5px;
  padding: 20px;
}

</style>

