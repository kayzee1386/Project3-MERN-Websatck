# Project3-MERN-Websatck

##This project taught me so many lessons;

1. Never to overflog an issue and seek for support where necessary.

2. Paying keen attention to every steps of the way. Follow instructions keenly!

3. Try again and keep trying. Don't ever **QUIT**


It was an interesting experience implementing project3 which covers the MERN (MongoDB, ExpressJS, React and NodeJS) web stack.

I signed in back into AWs and created an EC2 instance
![IP address](https://user-images.githubusercontent.com/46185705/131511722-3b48ec3f-5176-4be5-ad07-4edd485ff08a.jpg)

MobaXterm was launched and configured
![SSH-login](https://user-images.githubusercontent.com/46185705/131512061-64a1748f-c47f-4c61-a30d-e11e950dda4e.jpg)

Ubuntu was updated and upgraded as instructed in the documentation. After which NodeJs was installed as well.

Todo directory was created where ```npm init``` was initiated and i made use of ```ls``` to be sure that package.json file was created.

![package json](https://user-images.githubusercontent.com/46185705/131515581-7e8820a9-0e87-4b7a-95b9-b26d2019fa6a.jpg)

ExpressJS was later installed ```npm install express``` and a file of index.js was created as well.

![expressJS-installed](https://user-images.githubusercontent.com/46185705/131517291-fc07c926-3ba9-48a5-b1c2-dc2b757b5e6c.jpg)

Document environment was created with this command ```npm install dotenv``` and also port was also set to 5000

![server-running-5000-port](https://user-images.githubusercontent.com/46185705/131517921-9d6192c5-17b6-4bcd-8125-d72790e772a0.jpg)

The port was also established on security group of AWS instance created
![Ommited-this](https://user-images.githubusercontent.com/46185705/131518621-2efbf1cd-4792-4fef-ba97-21bf60caf38a.jpg)

```This step was omitted and it took me hours to figure it out```

Port figured out and Express installation was completed

![Welcome-to-Express](https://user-images.githubusercontent.com/46185705/131519434-fae880d8-e78a-41db-a83b-616e7fafc789.jpg)

Next: A new directory __route__ was created in the Todo directory and a file (api.js) was created with some code pasted in the new file.


#MODELS 
___
Models are used for defining the database schema. This is important so that we will be able to define the fields stored in each Mongodb document.

A model is at the heart of JavaScript based applications, and it is what makes it interactive.

Directory was changed back to Todo and ```npm install mongoose``` was run to install mongoose (Node.js package) to create a Schema and a model which makes working with mongodb easier.

![mongoose-installed](https://user-images.githubusercontent.com/46185705/131523529-26605186-6223-417f-8869-c4ece599c24a.jpg)

I `cd ..` into Todo and ran the following code `mkdir models && cd models && touch todo.js` with some code copied and pasted into api.js and todo.js respectively



                                                        ___MONGODB DATABASE___
_________

Database is needed to store our data and an account was created ***mLab*** for a free shared clustered account with AWS selected as the cloud provider.

![MongoDB-cluster](https://user-images.githubusercontent.com/46185705/131529367-c4e14785-8105-49c9-8e72-b3d89eac9227.jpg)
![mongodb-registration](https://user-images.githubusercontent.com/46185705/131529371-f015d7fd-3be9-49c4-80a0-ff7c3a407ce6.jpg)
![IP-mongoDB](https://user-images.githubusercontent.com/46185705/131529375-316cab7b-0355-4aa8-a371-8e07330088ec.jpg)

Server was started with `node index.js` with success message ___‘Database connected successfully’___ 

![DB-connected](https://user-images.githubusercontent.com/46185705/131530132-2704aa57-7d84-48a9-985c-bbc0061ebbb6.jpg)

```
Postman was installed to test all the API endpoints and make sure they are working end to end
```

![Postman-resolved](https://user-images.githubusercontent.com/46185705/131531021-580fe6e1-0730-4775-8f04-75639e6ea890.jpg)
![postman-get-response](https://user-images.githubusercontent.com/46185705/131531030-32d5796a-23a5-413f-9a7f-483e65ce787b.jpg)

![postman-delete](https://user-images.githubusercontent.com/46185705/131531498-a7a74793-b391-47a6-a724-17d882869fc5.jpg)


```
                                             ___FRONTEND CREATION___
```
React app client was created in the same directory as the Backend codes `Todo` using `npx create-react-app client`

![create-react-app](https://user-images.githubusercontent.com/46185705/131534817-1a86d9ed-c917-45a7-8a9b-244fae70b67c.jpg)

Some dependencies were installed and package.json file was updated with some code 
`npm install concurrently --save-dev` & `npm install nodemon --save-dev` 

![node_modules-installed](https://user-images.githubusercontent.com/46185705/131538689-079eb71a-98cc-4070-b87c-60eb81635e70.jpg)

key value pair was added to the package.json file `"proxy": "http://localhost:5000"` and the essense was to allow url on website

NOTE: Another TCP port was configured on AWS security group to allow access to 3000 port

App started running on localhost:3000 with this code `npm run dev`

                                                Creation of React Components

From Todo directory, i CDed into `clients/src` and inside `src` three files were created with this command `touch Input.js ListTodo.js Todo.js`. A code was copied and pasted into the Input.js file, after which AXIOS was installed with this command `npm install axios` but you have to be in clients directory to run the code. ` cd ../..` to be in the clients directory.

Go back to the components directory and open ListTodo.js file and paste below inside it

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

Likewise in `Todo.js` code below was copied and pasted as well

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

I went back to the src directory and deleted old code inside of the App.js file and pasted the code below in other to delete the logo

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

In the same `src` directory, App.css file was opened with `vi App.js` and below code was added as well. Save and exit

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

Also in the same `src` directory index.css file was openned and code below was pasted 

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

And finally, cd back into the Todo directory using `cd ../..` and run this command `npm run dev` and you should be able to run perfectly. See final output below;

![Completed](https://user-images.githubusercontent.com/46185705/131657607-4fbe659e-2b90-4492-82a0-873986350b9e.jpg)


```I love every obstacle i encountered on this project as i feel i've certainly added some knowledge to my trouble shooting skills```
          
                                                   ```Thank You```
