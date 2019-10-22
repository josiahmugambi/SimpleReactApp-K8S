
# Steps taken to create and deploy to Kubernetes Cluster on GCP

## Create React App
scroll down to see instructions

## Package into a Docker image
From within the root directory of the app create a Dockerfile (see https://docs.docker.com/engine/reference/builder/)
I used multi-stage build to optimize the size of the final container image. (h/t)

Also add a .dockerignore (works similar to .gitignore) in the same location and in it add

node_modules

## Run locally 
(This assumes you have docker installed on your laptop, if not, head over to https://docs.docker.com/install/ for instructions)

From within the root directory of the app, the basic command is: 

#### `sudo docker run -v ${PWD}:/app -v /app/node_modules -p 3001:3000 --rm a1pp:dev`

where:

a1pp:dev is your image tag

You should be able to view your app on http://localhost:3001 Port 3000 is what other containers would use to communicate with your app if you had more than one running.

### Upload the image to a registry
Since we are deploying to GCP, I opted to do a build using cloud build which also stores the image on google cloud registry
This can be done using the following command:

#### `gcloud builds submit --tag gcr.io/<your GCP project>/<your app>:latest`  

This assumes you have already setup gcloud SDK on your machine, and configured authentication, default project, zone and region. You'll also need to have enabled google cloud build on the specific project.



+++++++++++ Create React App instructions ++++++++++++

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
