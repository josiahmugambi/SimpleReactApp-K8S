
# Steps 

### 1. Create React App
scroll down to see instructions

### 2. Package into a Docker image
From within the root directory of the app create a Dockerfile (see https://docs.docker.com/engine/reference/builder/)
I used multi-stage build to optimize the size of the final container image. (h/t)

Also add a .dockerignore (works similar to .gitignore) in the same location and in it add

node_modules

### 3. Run locally 
(This assumes you have docker installed on your laptop, if not, head over to https://docs.docker.com/install/ for instructions)

From within the root directory of the app, the basic command is: 

##### `sudo docker run -v ${PWD}:/app -v /app/node_modules -p 3001:3000 --rm <your image name>:<tag>`


You should be able to view your app on http://localhost:3001 Port 3000 is what other containers would use to communicate with your app if you had more than one running.

### 4. Upload the image to a registry
Since we are deploying to GCP, I opted to do a build using cloud build which also stores the image on google cloud registry
This can be done using the following command:

##### `gcloud builds submit --tag gcr.io/<project id>/<your app>:latest`  

This assumes you have already setup gcloud SDK on your machine, and configured authentication, default project, zone and region. You'll also need to have enabled google cloud build on the specific project.

### 5. Create and Deploy kubernetes cluster on GCP

The GCP documentation will be useful here. I used the GC Console (faster for this purpose), though one can use gcloud 

I used f1-micro instances in a 3 node cluster (I'll delete the cluster in a few days) so as to keep the bill under the always free tier 

####  Configure kubectl credentials
##### `gcloud container clusters get-credentials <cluster name>`   

You can now view details of the cluster using  

##### `kubectl get <nodes/pods/services/deployments>`


#### Deploy app to cluster

##### `kubectl create deployment <deployment name> --image gcr.io/<project id>/<image name>:<tag>`

##### `kubectl expose deployment <deployment name> --type LoadBalancer --port 80` 

##### `kubectl get services` 

this should give you shomething similar to:
```
$ kubectl get services                                      (master✱)
NAME         TYPE           CLUSTER-IP   EXTERNAL-IP       PORT(S)        AGE
a1pp         LoadBalancer   10.0.5.63    104.198.131.155   80:30731/TCP   9m
kubernetes   ClusterIP      10.0.0.1     <none>            443/TCP        1h
```

Navigate to http://<external-ip-address> to view your deployed app

### Cloud Run ?
Reflection: doing this on Cloud Run (still in Beta) ought to be a lot simpler as a result of abstracting away the complexitiy of kubernetes - which is fun to learn and master if you're into infrastructure - but at times you just want to deploy an app in a hurry

The command would be something along the lines of:

##### `gcloud beta run deploy <service name> --region us-central1 --project <project id> --image gcr.io/<project id>/<image name>:<tag> --platform managed` 
  
  (you'd need to have beta components enabled via SDK, as well as the Cloud Run API via console)
  
## +++++++++++ Create React App instructions ++++++++++++

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.<br />
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.<br />
You will also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.<br />
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.<br />
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.<br />
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (Webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: https://facebook.github.io/create-react-app/docs/code-splitting

### Analyzing the Bundle Size

This section has moved here: https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size

### Making a Progressive Web App

This section has moved here: https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app

### Advanced Configuration

This section has moved here: https://facebook.github.io/create-react-app/docs/advanced-configuration

### Deployment

This section has moved here: https://facebook.github.io/create-react-app/docs/deployment

### `npm run build` fails to minify

This section has moved here: https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify
