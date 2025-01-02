require("dotenv").config();

const express = require("express");

const mongoose = require("mongoose");

const cors = require("cors");



const app = express();

app.use(cors());

app.use(express.json());



const uri = "mongodb+srv://db1:<db_password>@cluster1.foyjm.mongodb.net/myDatabase?retryWrites=true&w=majority";

mongoose

  .connect(uri, { useNewUrlParser: true, useUnifiedTopology: true })

  .then(() => console.log("Connected to MongoDB"))

  .catch((error) => console.error("MongoDB connection error:", error));



const ItemSchema = new mongoose.Schema({

  name: String,

  price: Number,

});



const Item = mongoose.model("Item", ItemSchema);



app.get("/", (req, res) => res.send("API is running"));



app.get("/items", async (req, res) => {

  try {

    const items = await Item.find();

    res.json(items);

  } catch (error) {

    res.status(500).json({ message: "Error fetching items" });

  }

});



app.post("/items", async (req, res) => {

  try {

    const newItem = new Item(req.body);

    await newItem.save();

    res.status(201).json(newItem);

  } catch (error) {

    res.status(400).json({ message: "Error adding item" });

  }

});

const PORT = process.env.PORT || 5000;

app.listen(PORT, () => console.log(`Server running on port ${PORT}`));




