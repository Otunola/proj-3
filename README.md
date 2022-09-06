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
      http://<PublicIP-or-PublicDNS>:5000
![Screen Shot 2022-09-06 at 1 49 31 PM](https://user-images.githubusercontent.com/112595648/188639509-8f7f15bb-38d1-4c3a-bd45-3b645629ccfe.png)





