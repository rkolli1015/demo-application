<img width="962" height="629" alt="image" src="https://github.com/user-attachments/assets/943e630e-4072-4587-95f1-a2767e10b5b1" /># demo-application
sample application

This project is a full‑stack blogging platform built with a React frontend and a Node.js/Express backend, containerized with Docker, and deployed on Amazon EKS (Elastic Kubernetes Service) using a Jenkins CI/CD pipeline.

Frontend
- React → Component‑based UI framework
- React Router → Client‑side routing
- Axios → API calls to backend
- Jest + React Testing Library → Unit and integration testing

Backend
- Node.js + Express → REST API server
- MongoDB / PostgreSQL → Database for users, posts, and comments
- JWT Authentication → Secure login and session handling
- Jest + Supertest → API testing

DevOps
- GitHub → Source control and collaboration
- Docker → Containerization of frontend and backend services
- Amazon ECR (Elastic Container Registry) → Store Docker images
- Jenkins → CI/CD pipeline (build, test, push, deploy)
- Amazon EKS (Elastic Kubernetes Service) → Kubernetes cluster for deployment
- Kubernetes → Orchestration, scaling, and service management
- kubectl → CLI for managing Kubernetes resources

Next 

i have created awsclasses directory in my local, in that i am placing all the code by creating folder name called blog-platform

Create frontend and backend folder
mkdir frontend
mkdir backend

Inside blog-platform, run:
New-Item -Name "Jenkinsfile" -ItemType File
New-Item -Name "k8s-deployment.yaml" -ItemType File
New-Item -Name "README.md" -ItemType File

Final Structure
our directory now looks like:

blog-platform/
├── frontend/
├── backend/
├── Jenkinsfile
├── k8s-deployment.yaml
└── README.md

Go into the frontend folde
cd frontend

next install the node.js
https://nodejs.org/en/download

select windows installer and do the setup

Add Node.js to PATH

Press Win + R, type:
sysdm.cpl
- and press Enter.
- Go to Advanced → Environment Variables.
- Under System variables, find Path → click Edit.
- Add a new entry:
C:\Program Files\nodejs\

- Click OK → close all dialogs.


Close your current PowerShell window completely.
Open a new one (normal user mode is fine)

**Verify Node and npm**
node -v
npm -v
npx -v

You should see version numbers (e.g., v24.x.x for Node, 11.x.x for npm).

if not , please fix the errors

next
Create React App

navigate to to your project 
cd C:\Users\kolli\awsclasses\blog-platform\frontend

create react app

Run
npx create-react-app .

. means create the app in the current folder

- Start the React development server

run 
npm start

Opens http://localhost:3000 in your browser.
You should see the React welcome page.

once you see the welcome page meanss React frontend is running successfully 🎉

next
open new powershell window and navigate to backend

cd C:\Users\kolli\awsclasses\blog-platform\frontend

Initialize Node.js project (only once):

npm init -y

Install Express:

npm install express

create app.js
New-Item -Name "app.js" -ItemType File

inside app.js 
paste the code

const express = require('express');
const app = express();
const PORT = 5000;

// Simple route
app.get('/', (req, res) => {
  res.send('Hello from Backend API!');
});

// Start server
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});

save it and 
run the backend
node app.js

you should see:
Server running on http://localhost:5000

next step is to connect your React frontend to the backend API so they can talk to each other

Install Axios in the frontend

Axios is a library that makes HTTP requests easy.
In PowerShell, go to your frontend folder:
cd C:\Users\kolli\awsclasses\blog-platform\frontend

run
npm install axios

if it shows any vulnerabilities run

run npm audit fix (This will update packages where fixes are available without breaking anything)

next edit the app.js file 

to add a /posts
update below code

const express = require('express');
const cors = require('cors');
const app = express();
const PORT = 5000;

// Enable CORS so frontend (port 3000) can call backend (port 5000)
app.use(cors());
app.use(express.json());

// Root route
app.get('/', (req, res) => {
  res.send('Hello from Backend API!');
});

// Posts route
app.get('/posts', (req, res) => {
  const posts = [
    { id: 1, title: 'First Post', content: 'This is my first blog post.' },
    { id: 2, title: 'Second Post', content: 'This is another blog post.' }
  ];
  res.json(posts);
});

// Test route
app.get('/test', (req, res) => {
  res.send('Test route working!');
});

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});

close the backend which you have created previously by CTRL+C

then restrt the backend

now open 
http://localhost:5000/posts

you will see the output

that JSON output means your /posts route is working exactly as intended! The backend is now serving blog post data.

in the same way open frontend/src/App.js

to connect this to your React frontend so the posts show up in the browser


paste the below code 
import { useEffect, useState } from 'react';
import axios from 'axios';

function App() {
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    axios.get('http://localhost:5000/posts')
      .then(response => {
        console.log(response.data); // Debugging: check what comes back
        setPosts(response.data);
      })
      .catch(error => console.error(error));
  }, []);

  return (
    <div>
      <h1>Blog Posts</h1>
      {posts.length === 0 ? (
        <p>No posts found.</p>
      ) : (
        <ul>
          {posts.map(post => (
            <li key={post.id}>
              <h2>{post.title}</h2>
              <p>{post.content}</p>
            </li>
          ))}
        </ul>
      )}
    </div>
  );
}

export default App;

then restart the frontend

cd C:\Users\kolli\awsclasses\blog-platform\frontend
npm start

test in browser open http://localhost:3000

you will see the output


**- Backend (Express) is serving posts at http://localhost:5000/posts.
- Frontend (React) is fetching that data and displaying it at http://localhost:3000.
- You can see your blog posts rendered correctly in the browser.
That means your frontend ↔ backend integration is complete. **



























