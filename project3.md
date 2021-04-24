## Simple To-Do application on MERN Web Stack

### Step 0
- Created a new EC2 instance with Ubuntu server 20.04 LTS (HVM) image.
- Downloaded MobaXterm and connected through SSH session to the Ubuntu server.

### Step 1 - Backend Configuration

- Updated ubuntu -  *sudo apt update*
- Upgrade ubuntu-   *sudo apt upgrade*
- Installed Node.js -  *sudo apt-get install -y nodejs*
- verified the node installation with  *node -v and npm -v*

![b](https://user-images.githubusercontent.com/81443311/115938407-703a9500-a492-11eb-81bd-b7d19a69b9f4.png)


- Created a new directory Todo - *mkdir Todo* 
- Ran command - *npm init* to initialise the project

![c](https://user-images.githubusercontent.com/81443311/115938557-01117080-a493-11eb-8111-2307f73b7405.png)


#### Installed ExpressJS 
- using *npm install express* and created a file index.js
- Installed dotenv module using *npm install dotenv*
- Opened index .js file and inserted code into it.
```
const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use((req, res, next) => {
res.send('Welcome to Express');
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
```

- Ran command - *node index.js*


- Created inbound rule for the EC2 instance by opening port 5000.

![e](https://user-images.githubusercontent.com/81443311/115943593-bd296600-a4a8-11eb-84ec-eacd1f5bbc82.png)

- Opened browser and accessed public IP via port 5000  *http://<PublicIP>:5000*
  
![F](https://user-images.githubusercontent.com/81443311/115943616-d92d0780-a4a8-11eb-98f0-5b4e7ef7735b.png)

#### Routes 
- Created  folder routes - mkdir routes and Created a file api.js in it.
- Inserted code into file api.js
```

const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;
```

![g](https://user-images.githubusercontent.com/81443311/115944000-34f89000-a4ab-11eb-8483-41444b8fac31.png)


#### Models
- Installed Mongoose in the Todo folder with -  npm install mongoose.
- Created new folder models - mkdir models and created a file todo.js in it.
- Inserted code into todo.js file.
```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
```
- Updated file api.js by deleting the code in the file and adding a new one.
```
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});

router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})

module.exports = router;
```
#### MongoDB Database

- Created a cluster 

![S14](https://user-images.githubusercontent.com/81443311/115944157-fadbbe00-a4ab-11eb-8a6c-cd5f3f431570.png)

- Updated the Network Access to allow access from anywhere and to run for 1week.

![S15](https://user-images.githubusercontent.com/81443311/115944172-1777f600-a4ac-11eb-8beb-1731b6c30412.png)

- Created database in collections 

![S16](https://user-images.githubusercontent.com/81443311/115944189-2eb6e380-a4ac-11eb-832f-794515b7b760.png)


- Created .env file in Todo directory and inserted the connection string to access the database.

-``*DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'*``
  
- Updated to:

-``*DB = mongodb+srv://olats:London251@cluster0.zyspf.mongodb.net/olatsDB?retryWrites=true&w=majority.*``

![h](https://user-images.githubusercontent.com/81443311/115944217-5a39ce00-a4ac-11eb-8c42-a6839ba05d3b.png)

- Updated the index.js file content by deleting and adding new code.

```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
console.log(err);
next();
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
```
- Started server with - node index.js
- Database connected successfully message observed.

![j](https://user-images.githubusercontent.com/81443311/115944254-910fe400-a4ac-11eb-809e-5915d8ac138b.png)

#### Testing Backend Code without Frontend using RESTful API

- Installed Postman 
- Created a POST request to the API on **http://54.83.77.54:5000/api/todos**

![k](https://user-images.githubusercontent.com/81443311/115944454-c2d57a80-a4ad-11eb-92df-1eea7c5dab54.png)

- Created  GET request to the API as above.

![l](https://user-images.githubusercontent.com/81443311/115944469-db459500-a4ad-11eb-913d-7d6911712cc4.png)

### STEP 2- Frontend creation

- Created new folder client in Todo directory   *npx create-react-app client*
- Installed concurrently; which is used to run multiple commands on the same terminal window 
*npm install concurrently --save-dev*
- Installed nodemon * npm install nodemon --save-dev*

![m](https://user-images.githubusercontent.com/81443311/115944628-c87f9000-a4ae-11eb-92b9-a0d3ca393109.png)

- substituted  part of the code in the package.json file (Todo folder)

```
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
```

#### Configure Proxy in package.json

- Opened package.json file in the client folder.
- Added the key value pair to the file contents -  ``"proxy": "http://localhost:5000"``

![o](https://user-images.githubusercontent.com/81443311/115944693-201dfb80-a4af-11eb-9a44-9988d2e22e31.png)

- In the Todo directory, i ran the *npm run dev*  command

![p](https://user-images.githubusercontent.com/81443311/115944892-9c650e80-a4b0-11eb-8ce1-1768c6b6c114.png)


- Created inbound rule for the EC2 instance by opening port:3000 


![q](https://user-images.githubusercontent.com/81443311/115944907-aedf4800-a4b0-11eb-8151-69889bfd23d1.png)


![r](https://user-images.githubusercontent.com/81443311/115944944-e948e500-a4b0-11eb-975c-acfe99c359b9.png)


#### Creating your React Components
- From Todo directory cd into client then cd into src 
- In src folder, created the components folder- mkdir components
- Created three files in the components folder: *Input.js ,  ListTodo.js ,  Todo.js*
- opened Input.js and inserted the code

```
import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {

state = {
action: ""
}

addTodo = () => {
const task = {action: this.state.action}

    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }

}

handleChange = (e) => {
this.setState({
action: e.target.value
})
}

render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>
)
}
}

export default Input
```

- In the clients folder, installed Axios - *npm install axios*

![t](https://user-images.githubusercontent.com/81443311/115945188-69237f00-a4b2-11eb-9d48-093c3195364b.png)


- Opened ListTodo.js in the components directory
- inserted code into the ListTodo.js file

```
import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {

return (
<ul>
{
todos &&
todos.length > 0 ?
(
todos.map(todo => {
return (
<li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
)
})
)
:
(
<li>No todo(s) left</li>
)
}
</ul>
)
}

export default ListTodo
```

- Inserted code into the Todo.js file 

```

import React, {Component} from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

state = {
todos: []
}

componentDidMount(){
this.getTodos();
}

getTodos = () => {
axios.get('/api/todos')
.then(res => {
if(res.data){
this.setState({
todos: res.data
})
}
})
.catch(err => console.log(err))
}

deleteTodo = (id) => {

    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))

}

render() {
let { todos } = this.state;

    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )

}
}

export default Todo;
```

- Moved into the src folder 
- Opened the App.js file and inserted code into it.
```
import React from 'react';

import Todo from './components/Todo';
import './App.css';

const App = () => {
return (
<div className="App">
<Todo />
</div>
);
}

export default App;

```

- Opened App.css file and inserted code

```

.App {
text-align: center;
font-size: calc(10px + 2vmin);
width: 60%;
margin-left: auto;
margin-right: auto;
}

input {
height: 40px;
width: 50%;
border: none;
border-bottom: 2px #101113 solid;
background: none;
font-size: 1.5rem;
color: #787a80;
}

input:focus {
outline: none;
}

button {
width: 25%;
height: 45px;
border: none;
margin-left: 10px;
font-size: 25px;
background: #101113;
border-radius: 5px;
color: #787a80;
cursor: pointer;
}

button:focus {
outline: none;
}

ul {
list-style: none;
text-align: left;
padding: 15px;
background: #171a1f;
border-radius: 5px;
}

li {
padding: 15px;
font-size: 1.5rem;
margin-bottom: 15px;
background: #282c34;
border-radius: 5px;
overflow-wrap: break-word;
cursor: pointer;
}

@media only screen and (min-width: 300px) {
.App {
width: 80%;
}

input {
width: 100%
}

button {
width: 100%;
margin-top: 15px;
margin-left: 0;
}
}

@media only screen and (min-width: 640px) {
.App {
width: 60%;
}

input {
width: 50%;
}

button {
width: 30%;
margin-left: 10px;
margin-top: 0;
}
}

```

- Opened index.css file and inserted code
```
body {
margin: 0;
padding: 0;
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
"Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
box-sizing: border-box;
background-color: #282c34;
color: #787a80;
}

code {
font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
monospace;
}
```

- In the Todo directory, ran code -  *npm run dev*


To-Do app now ready and fully functional with functionality such as : 
- Creating a task

![v](https://user-images.githubusercontent.com/81443311/115945610-69bd1500-a4b4-11eb-87ea-514ff820933b.png)

- Deleting a task 

![u](https://user-images.githubusercontent.com/81443311/115945549-31b5d200-a4b4-11eb-87e2-7ee6529a7781.png)

- Viewing all tasks.

![w](https://user-images.githubusercontent.com/81443311/115945626-780b3100-a4b4-11eb-8a27-33727012e0df.png)


                                      ## END OF PROJECT










 
 
