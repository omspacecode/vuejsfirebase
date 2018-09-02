<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>My First VueFire App (Real Time Database)</h1>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Add Book</h3>
        <div class="panel-body">
          <form id="form" class="form-inline" v-on:submit.prevent="addBook">
            <div class="form-group">
              <label for="bookTitle">Title:</label>
              <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
            </div>
            <div class="form-group">
              <label for="bookAuthor">Author:</label>
              <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">
            </div>
            <div class="form-group">
              <label for="bookUrl">URL:</label>
              <input type="text" id="bookUrl" class="form-control" v-model="newBook.url">
            </div>
            <input type="submit" class="btn btn-primary" value="Add Book">
          </form>
        </div>
      </div>
    </div>

    <div class="panel panel-default">
        <div class="panel-heading">
          <h3>Books Lists</h3>
        </div>
        <div class="panel-body">
          <table class="table table-striped">
            <thead>
              <tr>
                <th>Title</th>
                <th>Author</th>
                <th>Edit</th>
                <th>Delete</th>
              </tr>
            </thead>
            <tbody>

            <tr
            v-if="editId === true"
            class="collection-item "
            v-bind:class="{ 'yellow lighten-4': editId === true }"
          >
            <td>
              <input
                class="validate"
                v-model="editBookData.title"
              />  
            </td>
            <td>
              <input
                class="validate"
                v-model="editBookData.author"
              />  
            </td>
            <td>
              <input
                class="validate"
                v-model="editBookData.url"
              />  
            </td>
            <td>
              <span class="glyphicon glyphicon-ok" v-on:click="updateBook(editBookData)"></span>
            </td>
          </tr>


              <tr v-for="book in books">
                <td> <a v-bind:href="book.url" target="_blank"> {{ book.title }} </a></td>
                <td>{{ book.author }}</td>
                <td><span class="glyphicon glyphicon-edit" v-on:click="editBook(book)"></span></td>
                <td><span class="glyphicon glyphicon-trash" v-on:click="removeBook(book)"></span></td>  
              </tr>
            </tbody>
          </table>
        </div>
    </div>
  </div>
</template>

<script>

import Firebase from 'firebase'

let config = {
  apiKey: "AIzaSyAKB5zqBwd_UePHm72UTM8eo8Lmxc26pzk",
  authDomain: "vuejs-firebase-01-b554c.firebaseapp.com",
  databaseURL: "https://vuejs-firebase-01-b554c.firebaseio.com",
  projectId: "vuejs-firebase-01-b554c",
  storageBucket: "vuejs-firebase-01-b554c.appspot.com",
  messagingSenderId: "735896968925"
}

let app = Firebase.initializeApp(config);
let db = app.database();

let bookRef = db.ref('books');

export default {
  name: 'App',
  firebase: {
    books: bookRef
  },
  data () {
    return {
      newBook: {
        title: '',
        author: '',
        url: 'https://'
      },
      editId: '',
      editBookData: {
        title: '',
        author: '',
        url: ''
      }
    }
  },
  methods: {
    addBook: function (){
      bookRef.push(this.newBook);
      this.newBook.title = '';
      this.newBook.author = '';
      this.newBook.url = '';
    },
    removeBook: function (book){
      bookRef.child(book['.key']).remove();  
    },
    editBook: function (book) {
      this.editId = true;
      this.editBookData.key = book[".key"];
      this.editBookData.title = book.title;
      this.editBookData.author = book.author;
      this.editBookData.url = book.url;
    },
    updateBook: function (book) {
      bookRef.child(book.key).update({title: book.title, author: book.author, url: book.url});
      this.editId = '';
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
