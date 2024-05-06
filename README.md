animals-directory

app.js


// app.js
// Language: Node.js

const express = require('express');
const app = express();
const mongoose = require('mongoose');
const animalController = require('./controllers/animalController');

mongoose.connect('mongodb://localhost/animals-directory', { useNewUrlParser: true, useUnifiedTopology: true });

app.use(express.json());

app.get('/animals', animalController.getAllAnimals);
app.post('/animals', animalController.createAnimal);
app.get('/animals/:id', animalController.getAnimalById);
app.put('/animals/:id', animalController.updateAnimal);
app.delete('/animals/:id', animalController.deleteAnimal);

app.listen(3000, () => {
  console.log('Server started on port 3000');
});
controllers/animalController.js

markdown

// controllers/animalController.js
// Language: Node.js
javascript
Edit
Run
Full Screen
Copy code
const Animal = require('../models/Animal');

exports.getAllAnimals = async (req, res) => {
  try {
    const animals = await Animal.find().exec();
    res.json(animals);
  } catch (err) {
    console.error(err);
    res.status(500).json({ message: 'Error fetching animals' });
  }
};

exports.createAnimal = async (req, res) => {
  try {
    const animal = new Animal(req.body);
    await animal.save();
    res.json(animal);
  } catch (err) {
    console.error(err);
    res.status(400).json({ message: 'Error creating animal' });
  }
};

exports.getAnimalById = async (req, res) => {
  try {
    const id = req.params.id;
    const animal = await Animal.findById(id).exec();
    if (!animal) {
      res.status(404).json({ message: 'Animal not found' });
    } else {
      res.json(animal);
    }
  } catch (err) {
    console.error(err);
    res.status(500).json({ message: 'Error fetching animal' });
  }
};

exports.updateAnimal = async (req, res) => {
  try {
    const id = req.params.id;
    const animal = await Animal.findByIdAndUpdate(id, req.body, { new: true }).exec();
    if (!animal) {
      res.status(404).json({ message: 'Animal not found' });
    } else {
      res.json(animal);
    }
  } catch (err) {
    console.error(err);
    res.status(500).json({ message: 'Error updating animal' });
  }
};

exports.deleteAnimal = async (req, res) => {
  try {
    const id = req.params.id;
    await Animal.findByIdAndRemove(id).exec();
    res.json({ message: 'Animal deleted successfully' });
  } catch (err) {
    console.error(err);
    res.status(500).json({ message: 'Error deleting animal' });
  }
};
models/Animal.js

// models/Animal.js
// Language: Node.js

const mongoose = require('mongoose');

const animalSchema = new mongoose.Schema({
  name: { type: String, required: true },
  species: { type: String, required: true },
  age: { type: Number, required: true }
});

module.exports = mongoose.model('Animal', animalSchema);


Error handling: Implement a centralized error handling mechanism using a middleware function to catch and handle errors in a consistent manner.
Validation: Use a validation library like Joi or Express-validator to validate incoming requests and ensure data consistency.
Route organization: Consider organizing routes into separate files or modules to improve code organization and maintainability.
Security: Implement authentication and authorization mechanisms to restrict access to certain routes and ensure data security.
Testing: Write unit tests and integration tests to ensure the application's functionality and robustness.
