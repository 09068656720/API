# API

const express = require('express');
const app = express();
const mongoose = require('mongoose');
const dotenv = require('dotenv');
const authRoute = require('./routes/auth');
const verify = require('./verifyToken');

dotenv.config();

mongoose.connect(process.env.MONGO_URI)
  .then(() => console.log('Connected to DB!'))
  .catch(err => console.log(err));

app.use(express.json());

app.use('/api/user', authRoute);

app.get('/api/posts', verify, (req, res) => {
  res.json({
    posts: {
      title: 'My Private Post',
      description: 'You should only see this if you are logged in.'
    },
    user: req.user
  });
});

app.listen(process.env.PORT, () => console.log(`Server Up and Running on port ${process.env.PORT}`));
