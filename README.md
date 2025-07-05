# FollowBotWebsite
This is the website that will contain reliable and useful information about our FollowBot

# Configuration & Usage

## Front-end & Back-end
Developing on the front-end requires to set up the server side. And it's quite easy.

Firstly, we run our server with Node.js. Follow instructions [here](https://nodejs.org/en/download) to install.

* In your command prompt go to the `BotServer` directory
* Once in the directory you must run these pip commands:
  ```cmd
  pip install Flask
  pip install -U flask-cors
  ```

Our database is built using AWS DynamoDB. Install this package in order to access and interact with AWS services like DynamoDB and S3.
```
pip install boto3 
```
For more information about database interactions between our website and Arduino clients, go to [DatabaseDocument](./Documentation/DatabaseDocument.md):

* install these npm packages for front-end development:
```
pip install flask-login  # manage user sessions and authentication in Flask apps
npm install react-leaflet@4 leaflet  # build mapping interfaces
npm install chart.js react-chartjs-2 # interactive data visualizations
```


* Once those packages are installed alongside npm, you will need to do the next following commands:
```cmd
npm i
npm i -g npm
```
* Now after these above steps you are ready to build the front-end and back-end
```cmd
npm run build
```
* After the build, run the server
```cmd
npm run start
```

* Run this to render changes to the server for testing
```cmd
npm run watch
```

* In your browser be sure to use this url: `http://xx.xx.xx.xx:5000`
* the xx.xx.xx.xx is showed in the server command window.

## Other Documents
This will contain necessary documents that are needed for the website side of development. We have established the setup needed for the website, but we also include Documents aroun unit testing:

* [Jest Unit Testing](https://github.com/FrankVanris2/FollowBotWebsite/blob/main/Documentation/JestUnitTesting.md)
