# Vue Todo App

This application serves as an introduction to VueJS and Firebase.

## Resources

- [VueJS](https://vuejs.org/v2/guide/computed.html)
- [Firebase](https://firebase.google.com/)
- [VueFire](https://github.com/vuejs/vuefire)

## Setup

1. Include Resources

```
<!-- Vue -->
<script src="https://unpkg.com/vue"></script>
<!-- Firebase -->
<script src="https://gstatic.com/firebasejs/4.0.0/firebase.js"></script>
<!-- VueFire -->
<script src="https://unpkg.com/vuefire/dist/vuefire.js"></script>
```

2. Create a new `Vue` instance, and bind it to the `#app` element.

```
<div id="app">

</div>

<script>
  new Vue({
    el: "#app"
  });
</script>
```

3. Head to: https://console.firebase.google.com/, and 'Add a new project'.  Once created, click 'Add Firebase to your web app', and copy the initialization code - used in step 5.

4. From the Firebase Database console, navigate to 'Rules', and set both `read` and `write` to `true`.

```
{
  "rules": {
    ".read": "true",
    ".write": "true"
  }
}
```

5. Initialize Firebase and create a VueFire binding referencing Firebase stored todo data.

```
<script>
  var config = {
    apiKey: ,
    authDomain: ,
    databaseURL: ,
  };
  firebase.initializeApp(config);

  var todosRef = firebase.database().ref('todos');

  new Vue({
    el: "#app"
  });
</script>
```

6. Within the Vue instance, setup the data model for creating new todo objects, and reading back Firebase stored todo data.

```
<script>
  var config = {
    apiKey: "AIzaSyAap7WdFI_AmwuOHDrG8m6UzVYIcEcs2tM",
    authDomain: "vue-todo-ea6f5.firebaseapp.com",
    databaseURL: "https://vue-todo-ea6f5.firebaseio.com"
  };
  firebase.initializeApp(config);

  var todosRef = firebase.database().ref('todos');

  new Vue({
    el: "#app",

    data: {
      newTodo: {
        description: '',
        status: 'open'
      }
    },

    firebase: {
      todos: todosRef
    }
  });
</script>
```

7. Build method for creating a new todo.

```
<script>
  var config = {
    apiKey: "AIzaSyAap7WdFI_AmwuOHDrG8m6UzVYIcEcs2tM",
    authDomain: "vue-todo-ea6f5.firebaseapp.com",
    databaseURL: "https://vue-todo-ea6f5.firebaseio.com"
  };
  firebase.initializeApp(config);

  var todosRef = firebase.database().ref('todos');

  new Vue({
    el: "#app",

    data: {
      newTodo: {
        description: '',
        status: 'open'
      }
    },

    firebase: {
      todos: todosRef
    },

    methods: {
      createTodo: function() {
        todosRef.push(this.newTodo);
        this.newTodo.description = '';
      }
  });
</script>
```

8. Add form to markup for submitting a new todo

```
<div id="app">
  <form v-on:submit.prevent="createTodo">
    <input type="text" v-model="newTodo.description" placeholder="Todo Description">
    <input type="submit" value="Create Todo">
  </form>
</div>
```

9. Create markup for iterating through todos

```
<div id="app">
  <form v-on:submit.prevent="createTodo">
    <input type="text" v-model="newTodo.description" placeholder="Todo Description">
    <input type="submit" value="Create Todo">
  </form>

  <ul>
    <li v-for="todo in todos">
      <span>{{ todo.description }} - {{ todo.status }}</span>
    </li>
  </ul>
</div>
```

10. Add methods for updating and deleting todos
```
<script>
  var config = {
    apiKey: "AIzaSyAap7WdFI_AmwuOHDrG8m6UzVYIcEcs2tM",
    authDomain: "vue-todo-ea6f5.firebaseapp.com",
    databaseURL: "https://vue-todo-ea6f5.firebaseio.com"
  };
  firebase.initializeApp(config);

  var todosRef = firebase.database().ref('todos');

  new Vue({
    el: "#app",

    data: {
      newTodo: {
        description: '',
        status: 'open'
      }
    },

    firebase: {
      todos: todosRef
    },

    methods: {
      createTodo: function() {
        todosRef.push(this.newTodo);
        this.newTodo.description = '';
      },

      updateTodo: function(todo) {
        todosRef.child(todo['.key']).update({ status: 'closed' });
      },

      deleteTodo: function(todo) {
        todosRef.child(todo['.key']).remove();
      }
  });
</script>
```

11. Update markup to support updating and deleting todos

```
<div id="app">
  <form v-on:submit.prevent="createTodo">
    <input type="text" v-model="newTodo.description" placeholder="Todo Description">
    <input type="submit" value="Create Todo">
  </form>

  <ul>
    <li v-for="todo in todos">
      <span>{{ todo.description }} - {{ todo.status }}</span>
      <button v-on:click="updateTodo(todo)">&#10003;</button>
      <button v-on:click="deleteTodo(todo)">&times;</button>
    </li>
  </ul>
</div>
```
