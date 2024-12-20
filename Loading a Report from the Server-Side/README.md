# Loading a Report from the Server-Side
This example illustrates loading of the report from the server-side and saving the result to the mdc-file. 

### Installation and running
Use npm to install requred modules:

    $ npm install

Run Sample:

    $ npm start

### Step by step
Required and Stimulsoft Reports.JS modules loading:

    var http = require("http");
    var fs = require('fs');

    // Stimulsoft Reports module
    var Stimulsoft = require("stimulsoft-reports-js");

Define the accept() function that will process requests to the server and output the desired result at a specific URL address - an HTML page, script and style files, a rendered report:

    function accept(req, res) {
        //Send index.html
        if (req.url == "/") {
            res.writeHeader(200, { "Content-Type": "text/html" });
            res.end(fs.readFileSync("index.html"));
        }
        //Send reports.js
        else if (req.url == "/stimulsoft.reports.js") {
            res.writeHeader(200, { "Content-Type": "text/javascript" });
            res.end(fs.readFileSync("node_modules/stimulsoft-reports-js/Scripts/stimulsoft.reports.js"));
        }
        //Send viewer.js
        else if (req.url == "/stimulsoft.viewer.js") {
            res.writeHeader(200, { "Content-Type": "text/javascript" });
            res.end(fs.readFileSync("node_modules/stimulsoft-reports-js/Scripts/stimulsoft.viewer.js"));
        }
        //Send report
        else if (req.url == "/getReport") {
            // Creating new report
            var report = new Stimulsoft.Report.StiReport();
            // Loading report template
            report.loadFile("SimpleList.mrt");
            // Renreding report
            report.renderAsync(function () {
                // Saving rendered report to JSON string
                var reportJson = report.saveDocumentToJsonString();

                //Send report
                res.end(reportJson);
            });
        }
    }

Start the server and specify the required port, in this case, 8888:

    console.log("Static file server running at http://localhost:" + 8888 + "/\nCTRL + C to shutdown");
    //The HTTP server run on port 8888
    http.createServer(accept).listen(8888);
