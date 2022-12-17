# Didelo Front-End
Welcome to the Didelo front-end repository! This is the Vue 3 single-page application (SPA) that powers didelo.fr, a website for sharing lessons and homeworks with students.

## Prerequisites
To build and run the application, you will need the following:

Node.js 12.0 or higher
npm or yarn
## Installation
### Clone the repository:
```bash
git clone git@github.com:Didelo-lilian/lilian-front.git
```
### Install the dependencies:
```bash
cd lilian-front
npm install
```
or
```bash
cd lilian-front
yarn
```

### Create a .env file with the following variables:

```bash
VUE_APP_API_URL=https://api.didelo.fr
VUE_APP_API_KEY=your-api-key
```
Replace https://api.didelo.fr with the URL of your API, and your-api-key with your API key.

## Running the application
To start the development server, run the following command:

```bash
npm run serve
```
or
```bash
yarn serve
```
The application will be available at http://localhost:8080.

To build the application for production, use the build script:

```bash
npm run build
```
or

```bash
yarn build
```

To deploy the application to a live server, build the application using the build script, and then copy the contents of the dist folder to your server.

You may need to configure your server to serve the application as a static site. For example, with Nginx, you can add the following location block to your server configuration:

```nginx
location / {
try_files $uri $uri/ /index.html;
}
```

## Troubleshooting
If you encounter any issues or errors while running the application, please check the following:

Make sure that you have installed the required dependencies and configured the .env file correctly.
If you are experiencing issues with the API, make sure that the API is running and that you are using the correct API key.
If you are unable to resolve the issue, please open an issue in the issue tracker or contact us at <a href="mailto:lilian.soler@didelo.fr">lilian.soler@didelo.fr</a>.
