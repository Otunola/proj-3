# proj-3 : Implement a web solution based on MERN stack in AWS Cloud.
1. as usual we have to update and upgrade ubuntu with the commands : sudo apt update
2. and the command : sudo apt upgrade
3. Then Lets get the location of Node.js software from Ubuntu repositories with the command: curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
4. Install Node.js with the command below : sudo apt-get install -y nodejs
5. Verify the node installation with the command below : node -v 

![Screen Shot 2022-09-04 at 11 11 54 PM](https://user-images.githubusercontent.com/112595648/188335364-7b78044c-991e-4bfa-b2ce-cd2c89edf7ce.png)
6. then Create a new directory for your To-Do project with the command 
: mkdir Todo
7.you will use the command npm init to initialise your project, so that a new file named package.json will be created. This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. You can press Enter several times to accept default values, then accept to write out the package.json file by typing yes.
8. : npm init
![Screen Shot 2022-09-04 at 11 29 05 PM](https://user-images.githubusercontent.com/112595648/188335842-1aa7bca5-b6c2-4d3e-a0ad-59437364fe1c.png)
9. Run the command ls to confirm that you have package.json file created : ls
![Screen Shot 2022-09-04 at 11 31 46 PM](https://user-images.githubusercontent.com/112595648/188335900-58ac02ec-28a5-43b1-9e9e-6b4e2baa42ba.png)
10. Next, we will Install ExpressJs and create the Routes directory.

11. next, install express because Express helps to define routes of your application based on HTTP methods and URLs.
12. install it using npm: npm install express
13. create index.js file with : touch index.js
14. then install dotenv module : npm install dotenv
15. use vim to store the script below in the index.js : vim index.js
16. 
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
17. start our server to see if it works. Open your terminal in the same directory as your index.js file and type:
18. run the command : node index.js
19. below is the output
20. <img width="708" alt="Screen Shot 2022-09-05 at 10 37 23 PM" src="https://user-images.githubusercontent.com/112595648/188514874-b34e7f63-b341-489c-921d-b4dbbf7adcc4.png">
21. Now since node js is running on on port 5000, so we have to edit the inbound rule on the security group to allow traffic in port 5000 as shown below
![Screen Shot 2022-09-06 at 1 33 51 PM](https://user-images.githubusercontent.com/112595648/188636325-275936fb-05d3-49a4-9504-d20175dbc4bd.png)
22. Open up your browserand  access your server’s Public IP followed by port 5000:
23. 
      http://<PublicIP-or-PublicDNS>:5000
      
![Screen Shot 2022-09-06 at 1 49 31 PM](https://user-images.githubusercontent.com/112595648/188639509-8f7f15bb-38d1-4c3a-bd45-3b645629ccfe.png)
24. From the task, There are three actions that our To-Do application needs to be able to do: which are Create a new task, Display list of all tasks, Delete a completed task. and each of the tasks will be associated with HTTP request methods: POST, GET, DELETE.
      25. For each task, we need to create routes that will define various endpoints that the To-do app will depend on
26. we create routes folder with : mkdir routes
27. Change directory to routes folder: cd routes
28. create a file api.js : touch api.js
29. copy the following script and save : vim api.js
      
const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;

30. NOw we create a model. use models to define the database schema. To create a Schema and a model, install mongoose which is a Node.js package that makes working with mongodb easier.

31. Change directory back Todo : cd ..
32.  and install Mongoose : npm install mongoose
33. create a folder called 'models' : mkdir models
      34. change directory in to models : cd models
      35. create a file todo.js : touch todo.js     
      36. use vim to save the script below in todo.js : vim todo.js
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
      37. Now we need to update our routes from the file api.js in ‘routes’ directory to make use of the new model.
      38. open api.js with vim api.js, delete the code inside with :%d command and paste there code below into it then save and exit
      39. :
      cd ..
      cd routes
      vim api.js 
      paster and save the scrips below:
      
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
      40. next we setup our mongodb database using mLab for learning purposes. Follow the normal sign up process, select AWS as the cloud provider, and choose a region near you.
41. create your cluster
42. click on database access to add database user
43. click on network access to allow access to the database from some specific ip addresses
      44. go to collections and click on 'add my own data'
45. In the index.js file, we specified process.env to access environment variables, but we have not yet created this file. So we need to do that now.
46. Create a file in your Todo directory and name it .env and paste the following scripts:
      touch .env
       vi .env 
 47. and adjusting the username and password accordingly
DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
 48. Now we need to update the index.js to reflect the use of .env so that Node.js can connect to the database. Open the file with vim index.js
: vim index.js
 50. paste and save the code below
      const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
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
51. start your server useing : node index.js
      <img width="922" alt="Screen Shot 2022-09-06 at 9 05 14 PM" src="https://user-images.githubusercontent.com/112595648/188729835-e99545d1-fa37-4806-b885-a683369a64a7.png">
NOW OUR BACKEND IS SUCCESFULLY CONFIGURED
52. Testing Backend Code without Frontend using RESTful API using postman
      create a POST request to the API http://<PublicIP-or-PublicDNS>:5000/api/todos. This request sends a new task to our To-Do list so the application could store it in the database.
53. <img width="959" alt="Screen Shot 2022-09-06 at 9 57 38 PM" src="https://user-images.githubusercontent.com/112595648/188736754-03b5ece3-515f-4018-8b84-66e2e757491a.png">

54. Create a GET request to your API on http://<PublicIP-or-PublicDNS>:5000/api/todos. This request retrieves all existing records from out To-do application (backend requests these records from the database and sends it us back as a response to GET request
      and the response below was gotten
<img width="848" alt="Screen Shot 2022-09-06 at 9 58 32 PM" src="https://user-images.githubusercontent.com/112595648/188736882-4d4f68b7-7803-4b1d-bf13-b69dc974493e.png">

56. which shows that everything is ok
57. its time for frontend creation
58. it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, we will use the create-react-app command to scaffold our app.
59. In the root directory, run  : npx create-react-app client      
      the command will create a 'client'directory in the Todo directory
60. run : npm install concurrently --save-dev      
      which is used to run other commands concurrently
61. install Nodemon which is used to monitor the serder with the command : npm install nodemon --save-dev
62. In Todo folder open the package.json file. Change the highlighted part of the below screenshot and replace with the code below.
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
      
      ![Screen Shot 2022-09-08 at 11 00 40 AM](https://user-images.githubusercontent.com/112595648/189094331-1beb6faa-2c6b-424d-ad2e-43081df144b9.png)

62. : cd Todo
Open the package.json file

      : vi package.json 
      and do the edits
      
63.then we configure the proxy in package.json
      cd client
      vi package.json
  Then add Add the key value pair in the package.json file
      "proxy": "http://localhost:5000"
      this is to make the url accessible.

  64. Now Todo directory, and simply do:
       npm run dev
      
      <img width="1258" alt="Screen Shot 2022-09-08 at 10 27 52 AM" src="https://user-images.githubusercontent.com/112595648/189107613-108ebb1d-0327-41fc-a1c6-03e48784b4fb.png">
 We also allow the Ec2 security group instance to run traffic on port 3000
      
      ![Screen Shot 2022-09-08 at 4 51 34 PM](https://user-images.githubusercontent.com/112595648/189168107-4f1bbccd-0857-4720-9844-38b3fd4a4a8c.png)

      
      now we create react components
From your Todo directory run : cd client 
      66. move to the src directory  : cd src
      67. Inside your src folder create another folder called components:   mkdir components
      68. Move into the components directory with:  cd components
      69. In directory create three files Input.js, ListTodo.js and Todo.js. with command : touch Input.js ListTodo.js Todo.js
      70. Open Input.js file with command : vi Input.js       insert and paste the comand below  
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

      71. move to the client folder and install Axios with the command : npm install axios
      (Axios is Promise based HTTP client for the browser and node.js,)
      
     72. components’ directory with : cd src/components
      73.pen your ListTodo.js with :vi ListTodo.js
      74. then copy and pste the following code
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
      
      
74. in your Todo.js paste the following code:
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

export default Todo
      
      75. in the src directory, run : vi App.js
      then paste the following code: import React from 'react';

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
      76. in src directory open the App.css with commmand : vi App.css
      then paste the following script
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
 
      76. In the src directory open the index.css with : vim index.css
      then copy and paste the code beow
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
      77. Go to the todo directory and run : npm run dev
      theen you should have below
      <img width="1347" alt="Screen Shot 2022-09-08 at 5 25 57 PM" src="https://user-images.githubusercontent.com/112595648/189218175-134c7bba-51a9-4f01-b930-66242b0bc17e.png">
