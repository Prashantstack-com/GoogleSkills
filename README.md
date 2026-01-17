# IntroToDockerGoogle
> ⚠️ Note: Due to the Google Cloud Lab environment expiration, the Docker images and project files are not included.  
> This README documents all steps, commands, and outcomes of the lab. You can recreate the project by following these instructions.

 ## Overview
Hands-on lab to learn Docker basics: building, running, debugging, and publishing containers, showing portability with Google Artifact Registry.

## Quick Lab Steps

**1. Hello World:** 
`docker run hello-world && docker images && docker ps -a`  
**2. Build Node.js App:**  
Dockerfile: `FROM node:lts\nWORKDIR /app\nADD . /app\nEXPOSE 80\nCMD ["node","app.js"]`

app.js: `const http=require("http");http.createServer((req,res)=>{res.writeHead(200,{"Content-Type":"text/plain"});res.end("Hello World\n");}).listen(80,"0.0.0.0");`  

Build: `docker build -t node-app:0.1 .`  
**3. Run & Test:** 
`docker run -p 4000:80 --name my-app -d node-app:0.1 && curl http://localhost:4000`

**4. Update & Version:** change message to `"Welcome to Cloud"`, build: `docker build -t node-app:0.2 . && docker run -p 8080:80 --name my-app-2 -d node-app:0.2`  
**5. Push to Artifact Registry:** `gcloud auth configure-docker REGION-docker.pkg.dev && docker tag node-app:0.2 REGION-docker.pkg.dev/PROJECT_ID/my-repo/node-app:0.2 && docker push REGION-docker.pkg.dev/PROJECT_ID/my-repo/node-app:0.2`  

## Key Takeaways
Built, ran, and debugged Docker containers; managed versions and multiple containers; published images to Artifact Registry demonstrating portability.

> ⚠️ Note: Project files and images not included due to lab expiration. Steps above allow full recreation.
