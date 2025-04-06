Integration Guide: React ROI Calculator into Webflow (https://67f1e33e756bc2ecce762829--roi-calculator-easy-agile-1.netlify.app/)
This guide provides clear instructions on seamlessly integrating the React-based ROI Calculator into your Webflow website using an iframe. Additionally, it includes steps to hook up the form fields to store user data in your custom database.

Step 1: Hosting the React ROI Calculator
The React ROI Calculator has already been developed, and now you simply need to host it for integration. Follow these steps:

Host the React Build:

The React app is pre-built and ready to be hosted. Simply upload the contents of the build/ folder to your preferred hosting service. Recommended options include Netlify, Vercel, or AWS S3.

Upon uploading, you will receive a URL for the hosted app (e.g., https://yourapp.netlify.app).

Step 2: Embedding the ROI Calculator in Webflow
Once the React app is hosted, it can be easily embedded into your Webflow website using an iframe.

Embed the iframe:

Open your Webflow project and navigate to the page where you want to display the ROI Calculator.

Add an Embed element on the page and insert the following iframe code:

html
Copy
Edit
<iframe 
  src="https://yourapp.netlify.app" 
  width="100%" 
  height="600" 
  style="border: none;"
  title="ROI Calculator">
</iframe>
This will display the ROI Calculator within your Webflow site. The iframe can be resized as needed to fit the design.

Step 3: Connecting Form Data to the Backend
To capture and store the data from the ROI Calculator form into your database, we provide an API that seamlessly integrates with your backend.

Backend API Setup (Node.js + Express)
Install Required Packages: Ensure your backend is set up with the following dependencies:

bash
Copy
Edit
npm install express body-parser mongoose dotenv
Set up the API Endpoint: Create a simple Express API that receives the form data and stores it in your database. Here’s an example API setup:

javascript
Copy
Edit
const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');
require('dotenv').config();

const app = express();
app.use(bodyParser.json());

mongoose.connect(process.env.MONGODB_URI, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log("Connected to MongoDB"))
  .catch(err => console.error(err));

const roiSchema = new mongoose.Schema({
  employeeKeys: Number,
  employeeNum: Number,
  result: String
});

const RoiData = mongoose.model('RoiData', roiSchema);

app.post('/submit-roi', async (req, res) => {
  const { employeeKeys, employeeNum, result } = req.body;
  const newRoiData = new RoiData({ employeeKeys, employeeNum, result });

  try {
    await newRoiData.save();
    res.status(201).send('Data saved');
  } catch (err) {
    res.status(500).send('Error saving data');
  }
});

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
Sending Form Data from React to Your Backend
The React form fields will send the user input to the backend API. Use the following code to send the data after form submission:

javascript
Copy
Edit
const handleSubmit = async (formData) => {
  const response = await fetch('https://your-backend-url.com/submit-roi', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(formData)
  });

  if (response.ok) {
    console.log('Data saved successfully');
  } else {
    console.log('Failed to save data');
  }
};

// Example of the form data object
const formData = {
  employeeKeys: 10,
  employeeNum: 50,
  result: "Calculated ROI result"
};

handleSubmit(formData);
Step 4: Test and Confirm Integration
Once everything is set up:

Test the ROI Calculator: Navigate to the page where the iframe is embedded in Webflow and confirm the ROI Calculator is functioning as expected.

Verify Data Capture: Submit a test form through the ROI Calculator and ensure that the data is successfully sent to your backend and stored in the database.
