# gender-responsive-mechanism-of-domestic-voilence
const express = require('express');
const mongoose = require('mongoose');
const session = require('express-session');
const dotenv = require('dotenv');

dotenv.config();
const app = express();

// Connect MongoDB
mongoose.connect(process.env.MONGODB_URI, { useNewUrlParser: true, useUnifiedTopology: true });

// Middleware
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(session({
   secret: process.env.SESSION_SECRET,
   resave: false,
   saveUninitialized: false,
   cookie: { maxAge: 86400000 } // 1 day
}));

// Routes
const authRoutes = require('./routes/auth');
const articleRoutes = require('./routes/articles');
const monumentRoutes = require('./routes/monuments');
const placeRoutes = require('./routes/places');
const galleryRoutes = require('./routes/gallery');
const adminRoutes = require('./routes/admin');

app.use('/auth', authRoutes);
app.use('/api/culture', articleRoutes);
app.use('/api/monuments', monumentRoutes);
app.use('/api/places', placeRoutes);
app.use('/api/gallery', galleryRoutes);
app.use('/api/admin', adminRoutes);

// Serve frontend
app.use(express.static('frontend/public'));

// Start server
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
