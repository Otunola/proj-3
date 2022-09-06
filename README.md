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
