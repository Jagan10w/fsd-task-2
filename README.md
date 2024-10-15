const express = require('express');
const multer = require('multer');
const path = require('path');
const fs = require('fs');
const app = express();
const port = 3003;

const storage = multer.diskStorage({
destination: (req, file, cb) => {
const uploadDir = 'uploads/';
if (!fs.existsSync(uploadDir)) {
fs.mkdirSync(uploadDir);
}
cb(null, uploadDir);
},
filename: (req, file, cb) => {
cb(null, Date.now() + path.extname(file.originalname));
}
});
const upload = multer({ storage: storage });

app.get('/', (req, res) => {
res.send(`
<form action="/upload" method="post" enctype="multipart/form-data">
<input type="file" name="file">
<input type="submit" value="Upload">
</form>
`);
});

app.post('/upload', upload.single('file'), (req, res) => {
if (!req.file) {
return res.status(400).send('No file uploaded.');

}
res.send('File uploaded successfully!');
});
app.listen(port, () => {
console.log(`Server running at http://localhost:${port}`);
});
