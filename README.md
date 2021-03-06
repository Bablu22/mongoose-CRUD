# CRUD Application API using Express & Mongoose.

### What is Mongoose ?
Mongoose is an Object Data Modeling (ODM) library for MongoDB and Node.js. It manages relationships between data, provides schema validation, and is used to translate between objects in code and the representation of those objects in MongoDB.

## Features

- Post single data
- Post multiple data
- Get all data
- Get data by Id
- Update data by Id
- Delete data by Id


## Installation
Install the dependencies and devDependencies and start the server.

```sh
npm i express
npm i mongoose
npm i -D nodemon
```

## Development


#### Connect Mongoose 

```
const mongoose = require("mongoose")
mongoose.connect('mongodb://localhost:27017/todos')
    .then(() => {
        console.log("connectio success");
    })
    .catch(err => console.log(err))
```
Here we make a routeHandler file to manage all the todos route and export it form handlerRouter file. Thne require it in our root file index.js where we use it 
```
const routerHandler = require("./routeHandeer/routeHandler")
app.use("/todo", routerHandler)
```

Now in routeHandler file we manage our all route. But now we need a schema. Everything in Mongoose starts with a Schema. Each schema maps to a MongoDB collection and defines the shape of the documents within that collection.

```
const mongoose = require("mongoose")
const todoScema = mongoose.Schema({
    title: {
        type: String,
        required: true
    },
    description: String,
    date: {
        type: Date,
        default: Date.now
    }
})
module.exports = todoScema
```
Now we create a model in our handler file. Model name sould be start with uppercase. This model take two argument.
- Database name
- Schema 
```
const Todo = new mongoose.model("Todo", todosScema)
```
#### Get all todos
```
router.get("/", async (req, res) => {
    await Todo.find({}, (err, data) => {
        if (err) {
            res.status(500).json({
                error: "There was an server side error"
            })
        }
        else {
            res.status(200).json({
                result: data,
                message: "success"
            })
        }
    }).clone()
})
```
#### Get Todos by Id
```
router.get("/:id", async (req, res) => {

    await Todo.find({ _id: req.params.id }, (err, data) => {
        if (err) {
            res.status(500).json({
                error: "There was an server side error"
            })
        }
        else {
            res.status(200).json({
                result: data,
                message: "success"
            })
        }
    }).clone()
})
```

#### Post Single Todos
```
router.post("/", async (req, res) => {
    const newTodo = new Todo(req.body)
    await newTodo.save((err) => {
        if (err) {
            res.status(500).json({
                error: "There was an server side error"
            })
        }
        else {
            res.status(200).json({
                message: "Inserted successfully"
            })
        }
    })

})
```
#### Post Multiple Todos
```
router.post("/all", async (req, res) => {
    await Todo.insertMany(req.body, (err) => {

        if (err) {
            res.status(500).json({
                error: "There was an server side error"
            })
        }
        else {
            res.status(200).json({
                message: "Inserted successfully"
            })
        }
    })
})
```

#### Update Todos
```
router.put("/:id", async (req, res) => {
    await Todo.updateOne(
        { _id: req.params.id },
        {
            $set: {
                description: req.body.description
            }
        },
        (err) => {
            if (err) {
                res.status(500).json({
                    error: "There was an server side error"
                })
            }
            else {
                res.status(200).json({
                    message: "Updated successfully"
                })
            }
        }).clone()
})
```

#### Delete Todos
```
router.delete("/:id", async (req, res) => {
    await Todo.deleteOne({ _id: req.params.id }, (err, data) => {
        if (err) {
            res.status(500).json({
                error: "There was an server side error"
            })
        }
        else {
            res.status(200).json({
                result: data,
                message: "Delete successfully"
            })
        }
    }).clone()

})
```
# mongoose-grud
