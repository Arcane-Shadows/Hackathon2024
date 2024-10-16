
# Problem 1
Write a Python/Javascript code snippet to find the all the versions of the popular node.js express framework. Use the v3 version of the deps.dev API. You must provide functional code. Example output:
0.14.0, 0.14.1

# Solution
Here is the code:

const fetch = require('node-fetch');

async function fetchExpressVersions() {
    const url = 'https://deps.dev/api/v3/package/express';

    try {
        const response = await fetch(url);
        if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
        }

        const data = await response.json();
        const versions = Object.keys(data.versions);

        return versions.join(', ');
    } catch (error) {
        console.error('Error fetching versions:', error);
        return null;
    }
}

fetchExpressVersions().then(versions => {
    if (versions) {
        console.log(versions);
    }
});








# Problem 2
You developed a new package. You have named it "Epic". Over two years of arduous thankless open-source development you have released 3 versions of Epic on npm: 0.0.1, 0.0.2, 0.1.0. 0.1.0 is the default version.
Unfortunately, deps.dev doesn't return version information about "Epic". So, you wrote your own deps2.dev, which is exactly like deps.dev API (v3) except it works for "Epic" and only for "Epic". It returns information in exactly the same format.
Write a simple HTTP server for host deps2.dev. You don't need to use a database. You can just store the information in a variable.

# Solution
Step 1: Initialize

Create a new directory for the project and initialize it:

mkdir deps2dev
cd deps2dev
npm init -y


Step 2: Create the Server

Next, create a file named server.js and add the following code:

const http = require('http');

const epicPackageInfo = {
  name: "Epic",
  versions: {
    "0.0.1": {
      version: "0.0.1",
      dependencies: {},
      description: "Initial release of Epic."
    },
    "0.0.2": {
      version: "0.0.2",
      dependencies: {},
      description: "Minor improvements and bug fixes."
    },
    "0.1.0": {
      version: "0.1.0",
      dependencies: {},
      description: "First stable release with new features."
    }
  },
  defaultVersion: "0.1.0"
};


const server = http.createServer((req, res) => {
 
  res.setHeader('Content-Type', 'application/json');
  

  if (req.url === '/api/v3/package/Epic') {
    
    res.writeHead(200);
    res.end(JSON.stringify(epicPackageInfo));
  } else if (req.url === '/api/v3/package/Epic/versions') {
   
    res.writeHead(200);
    res.end(JSON.stringify(Object.keys(epicPackageInfo.versions)));
  } else {
   
    res.writeHead(404);
    res.end(JSON.stringify({ error: "Not Found" }));
  }
});

const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});


Now I can run the server using the following command:
node server.js








# problem 3
You run an e-commerce site. Your users constantly ask you the same questions on Facebook and WhatsApp, even though the answers are stated clearly on the product page and the FAQ section. To handle this, you want to develop a chatbot. Assuming you have an API key for a generative model, how will you implement the chatbot so that it can answer your user's questions accurately?

# solution

 We have to use Natural Language Processing(NLP) model which can be fine Tuned on our FAQ quenstions and answers or other product page related questions.

    user prompt -> fine tuned model's api -> generated output.

Or if we have an api key of a generative model, we can give it the FAQ and site related question answer then ask question, it will give the relevant answers.








# Problem 4
You want to create a smartphone app to detect ghosts  👻.
When a user opens your app it looks around using their camera. It then tells the user if there is a ghost around. If it finds a ghost, it identifies the type of the ghost and also highlights on their screen where the ghost is.
Brave users, who choose knowledge over safety,  can click on the ghost and learn more details about them. 
Now you might be wondering, how do I detect ghosts in the first place? 
Luckily for you, our team has already made an API for ghost detection, it's available on github.
Now tell us step-by-step how you will implement the app. You must cover the technical details. You can provide pseudo-code, but it's not necessary. 

# Solution

 Step 1: Set Up Development Environment

  -Platform Selection: Choose whether we want to develop the  app    for  iOS, Android, or both. For cross-platform development,   we can use frameworks React Native

  - Install Dependencies:
  - Camera and image processing libraries `react-native-camera` 
  - HTTP libraries to interact with the Ghost Detection  API `axios` 


 Step 2: Camera Integration

- Access the Camera: Use the device’s camera to continuously stream video data.
- Display the Camera Feed: Render the real-time camera feed on the screen, which will serve as the user interface for ghost detection.

  import { RNCamera } from 'react-native-camera';

  <RNCamera
    style={{ flex: 1 }}
    type={RNCamera.Constants.Type.back}
    captureAudio={false}
  />
 

Step 3: Ghost Detection via API

- Capture Frames: Periodically capture frames from the camera feed and send them to the Ghost Detection API for analysis.

- Ghost Detection API Call: The API from GitHub should accept image data and return the presence of a ghost, its type, and coordinates for where the ghost is located on the screen.

 Step 4: Display Ghost on Screen

- Ghost Overlay*: Once the API returns the location and type of ghost, overlay a marker (e.g., an icon or animated figure) at the specified coordinates on the camera feed to indicate the ghost's location.


<View style={{ position: 'absolute', top: ghostYPosition, left: ghostXPosition }}>
  <Image source={require('./ghost_icon.png')} style={{ width: 50, height: 50 }} />
</View>


 Step 5: Handle User Interaction

-Clickable Ghost: Allow the user to click on the ghost icon to get more information. Use a gesture detector or button to handle clicks.
  
<TouchableOpacity onPress={() => alert('Ghost Type: ' + ghost.type)}>
  <Image source={require('./ghost_icon.png')} style={{ width: 50, height: 50 }} />
</TouchableOpacity>


 Step 6: Ghost Details Page

- Details View: When the user clicks on the ghost, navigate them to a detailed page showing the ghost type and additional information. You can fetch more data from the API if available or store predefined details in the app.


navigation.navigate('GhostDetails', { ghost: ghost });


Step 7: Testing and Debugging

- Test on Real Devices: Since ghost detection relies on camera input, test our app on real devices to ensure proper functioning and alignment of ghost icons.
- Handle API Failures: Implement error handling for network issues and invalid responses from the Ghost Detection API.

 Step 8: Deployment

- App Store Submission: Once development and testing are complete, package and submit the app to app stores (Google Play, Apple App Store). Follow platform-specific guidelines for app publication.






# Problem 5
You might have noticed that all of the endpoints of Ghost-Detector is open for all. Lately we have noticed too many ghost and human sightings. We suspect that human users have started abusing the ghost endpoints and ghost users have started abusing the human endpoints. We need to prevent this!
Can you do that? If so, how?
Provide some code snippets with explanations and help us make our community of human and ghosts great again
# Solution


 Step 1: Implement Role-Based Access Control (RBAC)

We’ll assume there are two roles: "ghost" and "human". We'll restrict access to certain endpoints based on these roles.

1. Assign Roles: When users log in or interact with the system, we can assign them a role.
2. Verify Roles: On each API request, we will verify the user’s role and allow or deny access to endpoints accordingly.

 Step 2: Authentication (Using JWT Tokens)

We’ll use JWT (JSON Web Token) to handle user authentication. The JWT will include a claim that specifies whether the user is a "ghost" or a "human."



{
  "sub": "1234567890",
  "name": "John Doe",
  "role": "human",
  "iat": 1516239022
}



The `role` claim will help us enforce access control on the API endpoints.

Step 3: Middleware for Access Control

We’ll create a middleware function that checks the user’s role before allowing access to specific routes.

Example in Node.js using Express.js:



const jwt = require('jsonwebtoken');
function authorizeRole(requiredRole) {
  return (req, res, next) => {
    const token = req.headers['authorization'];
    if (!token) {
      return res.status(401).json({ message: 'Authorization token required' });
    }
    jwt.verify(token, 'your-secret-key', (err, decoded) => {
      if (err) {
        return res.status(403).json({ message: 'Invalid token' });
      }
      if (decoded.role !== requiredRole) {
        return res.status(403).json({ message: 'Access denied: You do not have the required permissions' });
      }

      next();
    });
  };
}



Step 4: Protect Endpoints Based on Role

We’ll now protect the ghost and human endpoints so that only users with the correct role can access them.

Example:




const express = require('express');
const app = express();


app.get('/ghost-endpoint', authorizeRole('ghost'), (req, res) => {
  res.json({ message: 'Welcome, ghost! You have access to this endpoint.' });
});


app.get('/human-endpoint', authorizeRole('human'), (req, res) => {
  res.json({ message: 'Welcome, human! You have access to this endpoint.' });
});


app.get('/general-endpoint', (req, res) => {
  res.json({ message: 'Everyone can access this endpoint.' });
});


app.listen(3000, () => {
  console.log('Server is running on port 3000');
});




 Step 5: Issuing Tokens Based on Role

When a user logs in or registers, the backend should generate a JWT with their assigned role (either ghost or human). Here's how you can issue a token:




function generateToken(user) {
  const payload = {
    sub: user.id,
    name: user.name,
    role: user.role 
  };

  return jwt.sign(payload, 'your-secret-key', { expiresIn: '1h' });
}

const user = { id: '123', name: 'Alice', role: 'ghost' };
const token = generateToken(user);
console.log(token); 



 Step 6: Securing Endpoints (Optional)

To enhance security, you can implement additional measures:
1. Rate Limiting: Limit the number of requests each user can make to prevent abuse.


 
   const rateLimit = require('express-rate-limit');
   const limiter = rateLimit({
     windowMs: 15 * 60 * 1000, 
     max: 100,
   });
   app.use(limiter);

  

2. CORS Protection: Ensure that only trusted domains can access the API.

 
   const cors = require('cors');
   const corsOptions = {
     origin: 'https://trusted-domain.com',
     optionsSuccessStatus: 200
   };
   app.use(cors(corsOptions));
  


Step 7: Logging and Monitoring

Track and log attempts to access restricted endpoints to monitor potential abuse:



app.use((req, res, next) => {
  console.log(`User with role ${req.user.role} accessed ${req.path}`);
  next();
});



 Step 8: Testing

Make sure to test the app thoroughly by:
- Generating tokens for both ghosts and humans.
- Making requests to the protected endpoints with valid and invalid roles.
- Monitoring for potential abuse and unauthorized access.

This solution will help you make the Ghost-Detector app more secure and ensure appropriate access for humans and ghosts alike!
