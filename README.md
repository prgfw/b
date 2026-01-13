# multer 
#single image upload 
1) const express = require("express");
const multer = require("multer");
const path = require("path");

const app = express();

// Storage configuration
const storage = multer.diskStorage({
    destination: function (req, file, cb) {
        cb(null, "uploads/");
    },
    filename: function (req, file, cb) {
        const uniqueName = Date.now() + "-" + file.originalname;
        cb(null, uniqueName);
    }
});

// File filter (only jpg and png allowed)
const fileFilter = (req, file, cb) => {
    const ext = path.extname(file.originalname).toLowerCase();
    if (ext === ".jpg" || ext === ".png") {
        cb(null, true);
    } else {
        cb(new Error("Only .jpg and .png files are allowed"), false);
    }
};

// Multer upload config
const upload = multer({
    storage: storage,
    fileFilter: fileFilter
});

// Route to upload single image
app.post("/upload", upload.single("profile"), (req, res) => {
    if (!req.file) {
        return res.status(400).json({ message: "No file uploaded" });
    }

    res.json({
        message: "File uploaded successfully",
        filename: req.file.filename,
        path: req.file.path
    });
});

// Error handling
app.use((err, req, res, next) => {
    if (err) {
        return res.status(400).json({ error: err.message });
    }
});

app.listen(3000, () => {
    console.log("Server running on port 3000");
});


2)
