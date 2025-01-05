================================================
File: /README.md
================================================
# Solar System NodeJS Application

A simple HTML+MongoDB+NodeJS project to display Solar System and it's planets.

---
## Requirements

For development, you will only need Node.js and NPM installed in your environement.

### Node
- #### Node installation on Windows

  Just go on [official Node.js website](https://nodejs.org/) and download the installer.
Also, be sure to have `git` available in your PATH, `npm` might need it (You can find git [here](https://git-scm.com/)).

- #### Node installation on Ubuntu

  You can install nodejs and npm easily with apt install, just run the following commands.

      $ sudo apt install nodejs
      $ sudo apt install npm

- #### Other Operating Systems
  You can find more information about the installation on the [official Node.js website](https://nodejs.org/) and the [official NPM website](https://npmjs.org/).

If the installation was successful, you should be able to run the following command.

    $ node --version
    v8.11.3

    $ npm --version
    6.1.0

---
## Install Dependencies from `package.json`
    $ npm install

## Run Unit Testing
    $ npm test

## Run Code Coverage
    $ npm run coverage

## Run Application
    $ npm start

## Access Application on Browser
    http://localhost:3000/



================================================
File: /Dockerfile
================================================
FROM node:18-alpine3.17

WORKDIR /usr/app

COPY package*.json /usr/app/

RUN npm install

COPY . .

ENV MONGO_URI=uriPlaceholder
ENV MONGO_USERNAME=usernamePlaceholder
ENV MONGO_PASSWORD=passwordPlaceholder

EXPOSE 3000

CMD [ "npm", "start" ]

================================================
File: /app-controller.js
================================================
console.log('We are inside client.js');

/* on page load  */
window.onload = function() {
    const planet_id = document.getElementById("planetID").value
    console.log("onLoad - Request Planet ID - " + planet_id)

    fetch("/os", {
            method: "GET"
        })
        .then(function(res) {
            if (res.ok) {
                return res.json();
            }
            thrownewError('Request failed');
        }).catch(function(error) {
            console.log(error);
        })
        .then(function(data) {
            document.getElementById('hostname').innerHTML = `Pod - ${data.os} `
          //  document.getElementById('environment').innerHTML = ` Env - ${data.env}  `
        });
};



const btn = document.getElementById('submit');
if (btn) {
    btn.addEventListener('click', func);
}

function func() {
    const planet_id = document.getElementById("planetID").value
    console.log("onClick Submit - Request Planet ID - " + planet_id)

    fetch("/planet", {
            method: "POST",
            body: JSON.stringify({
                id: document.getElementById("planetID").value
            }),
            headers: {
                "Content-type": "application/json; charset=UTF-8"
            }
        })
        .then(function(res2) {
            if (res2.ok) {
                return res2.json();
            }
            thrownewError('Request failed.');
        }).catch(function(error) {
            alert("Ooops, We have 8 planets.\nSelect a number from 0 - 8")
            console.log(error);
        })
        .then(function(data) {
            document.getElementById('planetName').innerHTML = ` ${data.name} `

            const element = document.getElementById("planetImage");
            const image = ` ${data.image} `
            element.style.backgroundImage  = "url("+image+")"

            const planet_description = ` ${data.description} `
            document.getElementById('planetDescription').innerHTML = planet_description.replace(/(.{80})/g, "$1<br>");

          
        });

}

================================================
File: /app-test.js
================================================
let mongoose = require("mongoose");
let server = require("./app");
let chai = require("chai");
let chaiHttp = require("chai-http");


// Assertion 
chai.should();
chai.use(chaiHttp); 

describe('Planets API Suite', () => {

    describe('Fetching Planet Details', () => {
        it('it should fetch a planet named Mercury', (done) => {
            let payload = {
                id: 1
            }
          chai.request(server)
              .post('/planet')
              .send(payload)
              .end((err, res) => {
                    res.should.have.status(200);
                    res.body.should.have.property('id').eql(1);
                    res.body.should.have.property('name').eql('Mercury');
                done();
              });
        });

        it('it should fetch a planet named Venus', (done) => {
            let payload = {
                id: 2
            }
          chai.request(server)
              .post('/planet')
              .send(payload)
              .end((err, res) => {
                    res.should.have.status(200);
                    res.body.should.have.property('id').eql(2);
                    res.body.should.have.property('name').eql('Venus');
                done();
              });
        });

        it('it should fetch a planet named Earth', (done) => {
            let payload = {
                id: 3
            }
          chai.request(server)
              .post('/planet')
              .send(payload)
              .end((err, res) => {
                    res.should.have.status(200);
                    res.body.should.have.property('id').eql(3);
                    res.body.should.have.property('name').eql('Earth');
                done();
              });
        });
        it('it should fetch a planet named Mars', (done) => {
            let payload = {
                id: 4
            }
          chai.request(server)
              .post('/planet')
              .send(payload)
              .end((err, res) => {
                    res.should.have.status(200);
                    res.body.should.have.property('id').eql(4);
                    res.body.should.have.property('name').eql('Mars');
                done();
              });
        });

        it('it should fetch a planet named Jupiter', (done) => {
            let payload = {
                id: 5
            }
          chai.request(server)
              .post('/planet')
              .send(payload)
              .end((err, res) => {
                    res.should.have.status(200);
                    res.body.should.have.property('id').eql(5);
                    res.body.should.have.property('name').eql('Jupiter');
                done();
              });
        });

        it('it should fetch a planet named Satrun', (done) => {
            let payload = {
                id: 6
            }
          chai.request(server)
              .post('/planet')
              .send(payload)
              .end((err, res) => {
                    res.should.have.status(200);
                    res.body.should.have.property('id').eql(6);
                    res.body.should.have.property('name').eql('Saturn');
                done();
              });
        });

        it('it should fetch a planet named Uranus', (done) => {
            let payload = {
                id: 7
            }
          chai.request(server)
              .post('/planet')
              .send(payload)
              .end((err, res) => {
                    res.should.have.status(200);
                    res.body.should.have.property('id').eql(7);
                    res.body.should.have.property('name').eql('Uranus');
                done();
              });
        });

        it('it should fetch a planet named Neptune', (done) => {
            let payload = {
                id: 8
            }
          chai.request(server)
              .post('/planet')
              .send(payload)
              .end((err, res) => {
                    res.should.have.status(200);
                    res.body.should.have.property('id').eql(8);
                    res.body.should.have.property('name').eql('Neptune');
                done();
              });
        });

        // it('it should fetch a planet named Pluto', (done) => {
        //     let payload = {
        //         id: 9
        //     }
        //   chai.request(server)
        //       .post('/planet')
        //       .send(payload)
        //       .end((err, res) => {
        //             res.should.have.status(200);
        //             res.body.should.have.property('id').eql(9);
        //             res.body.should.have.property('name').eql('Sun');
        //         done();
        //       });
        // });


    });        
});

//Use below test case to achieve coverage
describe('Testing Other Endpoints', () => {

    describe('it should fetch OS Details', () => {
        it('it should fetch OS details', (done) => {
          chai.request(server)
              .get('/os')
              .end((err, res) => {
                    res.should.have.status(200);
                done();
              });
        });
    });

    describe('it should fetch Live Status', () => {
        it('it checks Liveness endpoint', (done) => {
          chai.request(server)
              .get('/live')
              .end((err, res) => {
                    res.should.have.status(200);
                    res.body.should.have.property('status').eql('live');
                done();
              });
        });
    });

    describe('it should fetch Ready Status', () => {
        it('it checks Readiness endpoint', (done) => {
          chai.request(server)
              .get('/ready')
              .end((err, res) => {
                    res.should.have.status(200);
                    res.body.should.have.property('status').eql('ready');
                done();
              });
        });
    });

});

================================================
File: /app.js
================================================
const path = require('path');
const express = require('express');
const OS = require('os');
const bodyParser = require('body-parser');
const mongoose = require("mongoose");
const app = express();
const cors = require('cors')


app.use(bodyParser.json());
app.use(express.static(path.join(__dirname, '/')));
app.use(cors())

mongoose.connect(process.env.MONGO_URI, {
    user: process.env.MONGO_USERNAME,
    pass: process.env.MONGO_PASSWORD,
    useNewUrlParser: true,
    useUnifiedTopology: true
}, function(err) {
    if (err) {
        console.log("error!! " + err)
    } else {
      //  console.log("MongoDB Connection Successful")
    }
})

var Schema = mongoose.Schema;

var dataSchema = new Schema({
    name: String,
    id: Number,
    description: String,
    image: String,
    velocity: String,
    distance: String
});
var planetModel = mongoose.model('planets', dataSchema);



app.post('/planet',   function(req, res) {
   // console.log("Received Planet ID " + req.body.id)
    planetModel.findOne({
        id: req.body.id
    }, function(err, planetData) {
        if (err) {
            alert("Ooops, We only have 9 planets and a sun. Select a number from 0 - 9")
            res.send("Error in Planet Data")
        } else {
            res.send(planetData);
        }
    })
})

app.get('/',   async (req, res) => {
    res.sendFile(path.join(__dirname, '/', 'index.html'));
});


app.get('/os',   function(req, res) {
    res.setHeader('Content-Type', 'application/json');
    res.send({
        "os": OS.hostname(),
        "env": process.env.NODE_ENV
    });
})

app.get('/live',   function(req, res) {
    res.setHeader('Content-Type', 'application/json');
    res.send({
        "status": "live"
    });
})

app.get('/ready',   function(req, res) {
    res.setHeader('Content-Type', 'application/json');
    res.send({
        "status": "ready"
    });
})

app.listen(3000, () => {
    console.log("Server successfully running on port - " +3000);
})


module.exports = app;

================================================
File: /index.html
================================================
<html>
   <head>
      <link rel="preconnect" href="https://fonts.googleapis.com">
      <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
      <link href="https://fonts.googleapis.com/css2?family=Ubuntu&display=swap" rel="stylesheet">

      <link rel="preconnect" href="https://fonts.googleapis.com">
      <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
      <link href="https://fonts.googleapis.com/css2?family=Encode+Sans:wght@900&family=Ubuntu&display=swap" rel="stylesheet">

      <link rel="preconnect" href="https://fonts.googleapis.com">
      <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
      <link href="https://fonts.googleapis.com/css2?family=BioRhyme&family=Encode+Sans:wght@900&family=Ubuntu&display=swap" rel="stylesheet">

      <link rel="preconnect" href="https://fonts.googleapis.com">
      <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
      <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@900&display=swap" rel="stylesheet">

      <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">

      <meta name='viewport' content='width=device-width, initial-scale=1'>
      <title>Solar System - Sid</title>
      <link rel="icon" type="image/x-icon" href="https://gitlab.com/sidd-harth/solar-system/-/raw/main/images/saturn.png">
      <style> 
         #planetImage { 
            background: url('https://gitlab.com/sidd-harth/solar-system/-/raw/main/images/solar-system.png')   center center; 
            background-repeat: no-repeat;			   
            background-size: cover; 
            content: ''; 
            position: static; 
            animation: spin 25s linear infinite; 
            width: 50vw; 
            height: 50vw; 
         } 
         @keyframes spin { 
             100% { transform: rotate(360deg); } 
         } 
         body { 
            display: flex; 
            align-items: center; 
            justify-content: center; 
            background: url('images/background.gif'); 
         } 
         /*.shadow { 
         animation: rainbow 2s linear infinite; 
         } */ 
      </style>
   </head>
   <body>

      <div>
        <div>
         <a href="index.html">
            <button
               style=" font-size: 40px; 
                   background: rgb(50,43,167);
                   background: linear-gradient(90deg, rgba(50,43,167,1) 0%, rgba(82,41,124,1) 0%, rgba(137,26,205,1) 100%);
                   color:white;
                   font-family: 'Orbitron', sans-serif;
                   border-radius: 25px;     
                   border: 2px solid rgb(35, 34, 36);    
                   width: 600px; 
                   height: 70px;
                   text-align: center;
                   line-height: initial;
                   border-width: 1px 1px 3px"> SOLAR <i class="fa fa-rocket"></i> SYSTEM  </button> 
            </a>
           
        </div>
        <br>
         <input type="submit"  id="submit" value="Search the Planet" 
         style="float: right;
             background-color:rgb(187, 75, 243);
             color:white;
             font-family: 'Ubuntu';
             border-radius: 25px;     
             border: 2px solid #609;    
             padding: 20px; 
             width: 200px; 
             text-align:center;" /> 
         <div style="overflow: hidden; padding-right: .5em;" > 
            <input type="number" id="planetID" name="number" 
            style="width: 100%;
            background-color:rgb(218, 204, 226);
            text-align:center; 
            border-radius: 25px;     
            border: 2px solid #609;    
            padding: 20px;" placeholder="Enter a number(0 - 8) to view the planets"/> 
         </div>
         <div class="middle">
            <h1 style="color:rgb(247, 145, 95); font-family: 'Encode Sans';" id="planetName"> Solar System </h1>
         </div>
         <div class="bottomleft">
            <p  style="color:rgb(224, 224, 224); font-family: 'Ubuntu';" id="planetDescription"> Solar system consists of our star, the Sun, and everything bound to it by gravity – <br> the planets Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, and Neptune; <br> dwarf planets such as Pluto; dozens of moons; and millions <br> of asteroids, comets, and meteoroids.</p>
         </div>
         <br>
         <div class="middle">
            <h2 style="color:rgb(93, 150, 237); font-family: 'BioRhyme';"  id="hostname">Solar System Pod Name</h2>

            <!-- <h3 style="color:rgb(111, 255, 171); font-family: 'Encode Sans';" id="environment">ENVIRONMENT: " placeholder "</h3> -->
         </div>
      </div>
      <div  id="planetImage">  
      </div>
      <script type="text/javascript"  src="app-controller.js" charset="utf-8"></script>
   </body>
</html>

================================================
File: /package.json
================================================
{
  "name": "Solar System",
  "version": "6.7.6",
  "author": "Siddharth Barahalikar <barahalikar.siddharth@gmail.com>",
  "homepage": "https://www.linkedin.com/in/barahalikar-siddharth/",
  "license": "MIT",
  "scripts": {
    "start": "node app.js",
    "test": "mocha app-test.js --timeout 10000 --reporter mocha-junit-reporter --exit",
    "coverage": "nyc --reporter cobertura --reporter lcov --reporter text --reporter json-summary  mocha app-test.js --timeout 10000  --exit"
  },
  "nyc": {
    "check-coverage": true,
    "lines": 90
  },
  "dependencies": {
    "cors": "^2.8.5",
    "express": "^4.18.2",
    "mocha-junit-reporter": "^2.2.1",
    "mongoose": "5.13.20",
    "nyc": "^15.1.0"
  },
  "devDependencies": {
    "chai": "*",
    "chai-http": "*",
    "mocha": "*"
  }
}


================================================
File: /.dockerignore
================================================
# ---------------------------------------------------------------------------------*
# This will prevent your local modules and debug logs from being copied onto your 
# Docker image and possibly overwriting modules installed within your image.
# ---------------------------------------------------------------------------------*
node_modules
npm-debug.log
# ignore .git and .cache folders
.git
.cache
# ignore all markdown files (md) beside all README*.md other than README-secret.md
*.md
!README*.md
README-secret.md
# github related files
.github/
# nodejs releated files
node_modules
solar-system.png
.nyc_output
.talismanrc
coverage
test-results.xml

================================================
File: /kubernetes/development/deployment.yaml
================================================
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: solar-system
  name: solar-system
  namespace: _{_NAMESPACE_}_
spec:
  replicas: _{_REPLICAS_}_
  selector:
    matchLabels:
      app: solar-system
  strategy: {}
  template:
    metadata:
      labels:
        app: solar-system
    spec:
      containers:
      - image: _{_IMAGE_}_
        imagePullPolicy: Always
        name: solar-system
        ports:
        - containerPort: 3000
          name: http
          protocol: TCP
        envFrom:
        - secretRef:
            name: mongo-db-creds

================================================
File: /kubernetes/development/ingress.yaml
================================================
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: nginx
    kubernetes.io/tls-acme: "true"
  name: solar-system
  namespace: _{_NAMESPACE_}_
spec:
    rules:
    - host: solar-system-_{_NAMESPACE_}_._{_INGRESS_IP_}_.nip.io
      http:
        paths:
        - backend:
            service:
