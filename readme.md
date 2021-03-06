# qwidget-lineplot
Updated: Jul 18, 2016 by Ray Lai, Masaki Kakoi

## Summary
Line plot directive can display spacecraft vehicle's sensors, temperature, battery, and other telemetry information in real time. The design concept is inspired by GitHub projects angular-flot.

The angular-lineplot.js is an AngularJS directive that plots simple line graphs for different spacecraft telemetry information, though it is also re-usable for other charting requirements. The line graph data source can be either from data streaming (through WebSocket data streaming technology) or from database (via REST API). A standalone version is included in the /example subfolder. 

## Features
* Plot simple line graph
* Stream real-time data graph for spacecraft sensors or telemetry information via WebSocket
* Integrated with MongoDB database or any database via REST API

## Pre-requisites
* You need to install NodeJS on your target host (e.g. laptop, Linux host) first.
Using NodeJS's Node Package Manager, you can install this ground track widget. 

You can refer to the installation instructions under https://nodejs.org/en/download or https://nodejs.org/en/download/package-manager.

* You need "git" binaries installed on your target host. 
  - Git is pre-installed on MacOS.
  - On Linux host, you can install Git by "sudo yum install git" (for CentOS, Redhat, Fedora), or "sudo apt-get install git" (for Ubuntu).

* You need to create a local copy of this project. For example,
```
git clone https://github.com/quindar/qwidget-lineplot.git
``` 

## Dependencies
* AngularJS
* Flot (charts library)

Once you download the qwidget-lineplot project, you need to run buildme.sh in the example folder to install required module. Refer to the "How to Run the Demo" section for details. 	
	
## How to Run the Demo
* After creating a local copy of this project, run the script "buildme.sh" to install NodeJS dependencies and libraries:

```
./buildme.sh
```

If you use Windows machine, you can run the following commands as an alternative:
```
npm install
```

* Go to the example folder and run server.js to start the HTTP Web server: 
```
cd example
node server.js
```

You can also use:
```
nodemon server.js
```

The utility "nodemon" is similar to "node" (HTTP Web server), and it will automatically reload the Web pages whenever any Web page is updated.

Open a Web browser with the URL http://localhost:3000. You should see a Web page with a line graph.
You can click "Stream" to start streaming satellite telemetry data at real time.
If you can click "Load" and then "Plot", you can render historic satellite telemtry data.

From the top left corner, you can select the satellite spacecraft (e.g. Audacy1, Audacy2) and the telemetry data types (e.g. x, y, z are position telemetry data points).

## How to Integrate with Quindar
Quindar is a real-time mission operations application produced by Audacy. You can add this lineplot directive to grid-like window in Quindar as per the following steps:
	
* Create a copy of quindar-ux-angular on your target host 
  - e.g. git clone https://github.com/quindar/quindar-ux-angular.git)
* Create a copy of qwidget-lineplot on your target host under a separate folder.
* Copy the file quindar-lineplot.js from qwidget-lineplot project to quindar-ux-angular project
  - From qwidget-lineplot project folder "/dist" (https://github.com/quindar/qwidget-lineplot/tree/master/dist) 
  - To the quindar-ux-angular project folder "/app/directives".
* Copy the file app-lineplot.js from qwidget-lineplot project to quindar-ux-angular project.
  - From qwidget-lineplot project folder "/example/app/controllers" (https://github.com/quindar/qwidget-lineplot/tree/master/example/app/controllers)
  - To quindar-ux-angular project folder "/app/controllers"
* Edit the quindarWidgetsControllers.js (controller) to add the new widget qwidget-lineplot:
  - Add your widget definition in the $scope.widgetDefinitions:
```
   var widgetDefinitions = [
      {
        name: 'Line Plot',
        directive: 'lineplot',
        style: {
          width: '33%'
        }
      },
      {
        name: 'Ground Track',
        directive: 'groundtrack',
        style: {
          width: '100%'
        }
      }
    ];
```

* Add your new widget in a page definition (e.g. page 1 with id=0) in the $scope.dashboards array, e.g.
```
$scope.dashboards = [
      {
        id: 0,
        name: 'Basic',
        widgets: [{
          col: 0,
          row: 0,
          sizeY: 3,
          sizeX: 4,
          name: "Page 1 - Line Plot",
          directive: "lineplot"
        }]
      },
      ...
      {
        id: 4,
        name: 'Custom',
        widgets: []
      }
    ];
```

This will enable Quindar widget to render lineplot widget on page 1, by specifying the directive name "lineplot". 

* Add the controller quindarWidgetsControllers.js to include angular-lineplot JavaScript files. Here is an example of the changes:
  - Copy the lineplot controller file (e.g. app-lineplot.js) to the folder /app/controllers.
  - Modify the data source (e.g. server endpoint URL) in the app-lineplot.js as appropriate.
  - Modify quindarWidgetsControllers.js to add lineplot directive to the angular.module, e.g. var app = angular.module("app", ['gridster', 'ui.bootstrap', 'ui.router','angular-groundtrack','d3','angular-lineplot']);
  - Comment out scopes from angular-lineplot.js wherever appropriate.
  - Remove a dependency from app-lineplot.js, e.g. var app = angular.module('app')

* Update the JavaScript and CSS stylesheet in the file index.html
  - Your new AngularJS directive probably requires new JS/CSS files. You may want to review the current index.html
to see if the versions are compatible.
  - qwidget-lineplot requires angular-flot third party JS/CSS. They are consolidated and concatenated in the files "lineplot-thirdparty.js" and "lineplot-thirdparty.css" for convenience. Refer to https://github.com/quindar/qwidget-lineplot/tree/master/example/dist for details.
  - You can refer to the /example/index.html as an example.
  - e.g. for qwidget-lineplot project, you will need to add the following files:
```
<!-- custom JS for lineplot -->
  <script src="app/js/jquery.flot.resize.js"></script>
  <script src="app/js/jquery.flot.axislabels.js"></script>
  <script src="app/js/jquery.flot.navigate.js"></script>
  <script src="app/js/jquery.flot.selection.js"></script>
  <script src="app/js/jquery.flot.time.js"></script>
	
  <script src="dist/angular-lineplot.js"></script>
  <script src="app/controllers/app-lineplot.js"></script>
  <script src="config/clientSettings.js"></script>
```

* You can manually re-test your new quindar-ux-angular mission operations application to verify if the application works as expected.
  - There will be some automated widget test scripts in the quindar-ux-angular project.
  - You can run "nodemon server.js" and open a Web browser with the URL http://localhost:3000 to test the changes.

# Known Constraints
* No known issue so far.

