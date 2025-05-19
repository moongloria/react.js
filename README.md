# react.js
react
Try AI directly in your favorite apps … Use Gemini to generate drafts and refine content, plus get Gemini Advanced with access to Google’s next-gen AI for US$19.99 $0 for 1 month
const express = require('express');
const mysql = require('mysql2');
const cors = require('cors');
const bodyParser = require('body-parser');

const app = express();
app.use(cors());
app.use(bodyParser.json());

// Connect to MySQL
const db = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: '',     
  database: 'userdb'
});

db.connect(err => {
  if (err) {
    console.error('Database connection error:', err);
  } else {
    console.log('Connected to MySQL database');
  }
});

// Endpoint to handle Register
app.post('/register', (req, res) => {
  const { username, password } = req.body;
  const sql = 'INSERT INTO users (username, password) VALUES (?, ?)';
  db.query(sql, [username, password], (err, result) => {
    if (err) {
      console.error('Error inserting user:', err);
      res.status(500).send('Server error');
    } else {
      res.send('User registered successfully');
    }
  });
});

// Endpoint to handle Login
app.post('/login', (req, res) => {
  const { username, password } = req.body;
  const sql = 'SELECT * FROM users WHERE username = ? AND password = ?';
  db.query(sql, [username, password], (err, result) => {
    if (err) {
      console.error('Login error:', err);
      return res.status(500).send('Server error');
    }
    if (result.length > 0) {
      res.send('Login successful!');
    } else {
      res.status(401).send('Invalid Username or Password');
    }
  });
});

// Get/View/Display all users
app.get('/users', (req, res) => {
  db.query('SELECT * FROM users', (err, results) => {
    if (err) {
      console.error('Error fetching users:', err);
      res.status(500).send('Server error');
    } else {
      res.json(results);
    }
  });
});

// Delete User By ID
app.delete('/delete/:id', (req, res) => {
  const userId = req.params.id;
  db.query('DELETE FROM users WHERE id = ?', [userId], (err, result) => {
    if (err) {
      console.error('Delete error:', err);
      res.status(500).send('Server error');
    } else {
      res.send('User deleted successfully');
    }
  });
});

// Get User By ID
app.get('/user/:id', (req, res) => {
  const id = req.params.id;
  db.query('SELECT * FROM users WHERE id = ?', [id], (err, result) => {
    if (err) {
      res.status(500).send('Error fetching user');
    } else {
      res.json(result[0]);
    }
  });
});

// Update User By ID
app.put('/update/:id', (req, res) => {
  const id = req.params.id;
  const { username, password } = req.body;

  db.query('UPDATE users SET username = ?, password = ? WHERE id = ?', [username, password, id], (err, result) => {
    if (err) {
      console.error(err);
      res.status(500).send('Error updating user');
    } else {
      res.send('User updated successfully');
    }
  });
});


// Start server
app.listen(3001, () => {
  console.log('Server running on http://localhost:3001');
});
