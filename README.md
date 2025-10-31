# 🚀 Node.js + MongoDB Project Setup Guide

Follow the steps below carefully to set up your project from scratch.

---

## 🧩 Step 1: Open Code Editor

* Open **VS Code** (or any preferred code editor).
* Add your **project root folder** to the workspace.

---

## 🧮 Step 2: Open Terminal

Inside VS Code, open the terminal by pressing:

```
Ctrl + `
```

Then run the following commands one by one 👇

### Check Current Execution Policy

```powershell
Get-ExecutionPolicy -List
```

### Allow Script Execution for Current User

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

Press **Y** when prompted.

---

## ⚙️ Step 3: Initialize Node.js Project

Run this command to initialize your project:

```bash
npm init
```

Enter project details like:

* **name**
* **author**
* **version**
* **description**

Or just press **Enter** repeatedly to accept defaults.

---

## 📦 Step 4: Install Required Modules

Install basic dependencies:

```bash
npm install express cors http mongoose nodemon
```

---

## 🗂️ Step 5: Create Folder Structure

Your folder structure should look like this:

```
root/
 ├── model/
 │   └── User.js
 ├── package.json
 └── server.js
```

---

## ✏️ Step 6: Create Model File — `model/User.js`

```js
const mongoose = require('mongoose')

const UserSchema = new mongoose.Schema({
    name: {
        type: String,
        required: true
    },
    rollno: {
        type: String,
        required: true
    },
    email: {
        type: String,
        required: true
    },
    tp: {
        type: Number,
        default: 50
    },
})

module.exports = mongoose.model("user", UserSchema);
```

---

## 🧠 Step 7: Create Main Server File — `server.js`

```js
const express = require('express');
const cors = require('cors');
const mongoose = require('mongoose');
const http = require('http');

const app = express();
const server = http.createServer(app);

app.use(cors());
app.use(express.json());

const UserSchema = require('./model/User');

const PORT = 5000;

app.get('/', (req, res) => {
    res.send("This is the Home Route");
});

app.post('/register', async (req, res) => {
    const { name, email, rollno, tp } = req.body;
    const Student = new UserSchema({
        name: name,
        rollno: rollno,
        email: email,
        tp: tp
    });

    const RegisterStudent = await Student.save();
    if (RegisterStudent) {
        res.send({ message: "Student Registered Successfully!" });
    }
});

mongoose.set('strictQuery', false);

const connection = async () => {
  try {
    await mongoose.connect("mongodb://localhost:27017/Train", {
      serverSelectionTimeoutMS: 15000,
      socketTimeoutMS: 20000
    });
    console.log('Database Connection Successful ✅');
  } catch (err) {
    console.error('Database Not Connected ❌', err.message);
    process.exit(1);
  }
};

connection().then(() => {
    server.listen(PORT, () => {
        console.log(`Server running at Port : ${PORT}`);
    });
});
```

---

## 🧪 Step 8: Run the Server

Start your Node.js project with:

```bash
node server.js
```

or (for live reloading)

```bash
npx nodemon server.js
```

✅ If everything is correct, you’ll see:

```
Database Connection Successful ✅
Server running at Port : 5000
```

---

## 🎯 Done!

Your project is now ready.
You can now test it using **Postman** or any API testing tool.

**Example Test Route:**
`GET http://localhost:5000/`
