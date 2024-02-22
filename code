const express = require('express');
const mysql = require('mysql');
const fs = require('fs');
const { createCanvas, loadImage } = require('canvas');
const app = express();
// Create a connection to the MySQL database
const connection = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: 'password',
    database: 'charan'
});
// Connect to MySQL
connection.connect(err => {
    if (err) {
      console.error('Error connecting to MySQL database: ' + err.stack);
      return;
    }
    console.log('Connected to MySQL database.');
  });
// Read the image file
const imagePath = 'C:/Users/USER/Downloads/Screenshot 2024-02-16 201946.png';
const image = fs.readFileSync(imagePath);
// Insert the image into the database
const sql = 'INSERT INTO images (image_data) VALUES (1)';
connection.query(sql, [image], (error, results, fields) => {
   if (error) {
       console.error('Error inserting image:', error);
        return;
  }
  console.log('Image inserted successfully');
});
app.get('/image/:id', (req, res) => {
    const imageId = req.params.id;
    const sql = 'SELECT image_data FROM images WHERE id = 1';
    connection.query(sql, [imageId], (error, results, fields) => {
        if (error) {
            console.error('Error retrieving image:', error);
            res.status(500).send('Error retrieving image');
            return;
        }
        if (results.length === 0) {
            res.status(404).send('Image not found');
            return;
        }
        const imageData = results[0].image_data;
        res.writeHead(200, {'Content-Type': 'image/jpeg'});
        res.end(imageData);
    });
});
try {
    // Convert the Blob image data to a JPEG image
    const image = loadImage(imageData);
    const canvas = createCanvas(image.width, image.height);
    const ctx = canvas.getContext('2d');
    ctx.drawImage(image, 0, 0, image.width, image.height);

    // Send the JPEG image as the response
    res.writeHead(200, { 'Content-Type': 'image/jpeg' });
    canvas.createJPEGStream().pipe(res);
  } catch (error) {
    console.error('Error converting to JPEG:', error);
    res.status(500).send('Internal Server Error');
  }
// Start the Express.js server
const port = 4000;
app.listen(port, () => {
    console.log(`Server is listening on port ${port}`);
});
