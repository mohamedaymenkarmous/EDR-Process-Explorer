# EDR-Process-Explorer
This project shows a graphical view of the process executions relationship in a tree format (HTML version).
This is a major improvement of [adamfeuer's first version of the "D3JS Tree Visualizer with Select2-based Search"](https://gist.github.com/adamfeuer/07a4fb2bf5c52c039e7c5f17cf4d22e5). Many thanks to his efforts.

# Why do we need this?
This project is useful when:
* The EDR (Endpoint Detection and Response) has a retention limitation that forced your company to ingest the logs in a log aggregator or in a SIEM where it's painful to represent the process tree outside of the EDR platform where the data is no longer available in the EDR.
* The EDR platform doesn't show all the logged events in their graphical process explorer. The graphical interface of the EDR process explorer can be seen as amazing but unfortunately, not all the events are shown there.

# Setup
The web page can work only inside a web service. You need to setup the web service in python using the following command:
```python
cd ~/Documents
git clone https://github.com/mohamedaymenkarmous/EDR-Process-Explorer
# it's important to run the web service from the src directory
cd src
python3 -m http.server
```
The best way to make this project work properly is to use it locally. This is because the process execution logs can be voluminous. The web page will load the process tree located in the `src/flare.json` file. If your internet connection is slow, it'll not be practical to upload that JSON file to a remote server and then reload the web page and wait for a while until that JSON file is loaded and the web page is rendered.

Once you run the web service, you can access the web page from [http://localhost:8000](http://localhost:8000). In order to see a graphical process tree, you need to make sure the `src/flare.json` file is created.

# Example
When you access the web page delivered through the web service, you'll see the parent process tree only. You can expand any branch and you cal also search for any indicator that was seen in the process details or their context. Using the search will make it expand all the parent nodes of the searched items and it will make that path colored in red.
This was a test performed in my sandbox:
<img src="https://github.com/mohamedaymenkarmous/EDR-Process-Explorer/blob/main/example.png">
The data being generated was modified and saved in this file [src/flare.json](src/flare.json).

This is just an example of 1 single process tree. But, in practice when you extract the process logs from a host between two different dates, you'll find several independant processes that you can include separately under the root node since the format of the [src/flare.json](src/flare.json) file should be in JSON format based on a JSON object (the parent structure should not be a list of objects). This is why, I adopted the structure of a parent "Independant process" that is the parent of multiple independant processes (1, 2, 3, etc).

# Demo
You can access find the example mentioned above [here in a live demo](https://jsfiddle.net/TheEmperors/vo7m1d6a/2/show/)

# Features
Following are the features that are implemented or in the roadmap of the implementation:
- [x] The process tree should be in the proper format: JSON (originally implemented)
- [x] Draw the process tree in the web page (originally implemented)
- [x] Add a search bar that shows the location of the searched process (this basic feature is originally implemented)
  - [x] Make the search search for the process' context without showing those details in the dropdown menu (e.g. search a domain name that could be resolved by a process or used somehow by a process) (new)
  - [x] Make the search show beautified format of the process details using HTML tags. This allows the data included in the `src/flare.json` file to be in HTML format (new)
  - [x] Make the search list all the processes including the parent processes and the children processes) (new)
  - [x] Make the search color the nodes with hidden children differently from the rest of the nodes (new)
- [x] Add a "process details and context" section where every node that is hovered with the mouse will show the process details and context in that section (new)
- [x] Adjust the dimensions of the graph to take advantage of 100% of the width and 100% of the rest of the height (new)
- [x] Add a Zoom feature to zoom in and out from the graph (new)
- [x] Add a Drag and move feature to move freely inside the graph even if the graph is bigger than the screen (without a scroll bar) (new)
- [x] Add an automatic adjustment of the graph's height based on the number of children seen in the expanded or collapsed section
- [ ] Make the view stay in the same location if a node is expanded or collapsed (due to the fact that the height is automatically adjusted, that will make the nodes adjust their Y-coordinates and this can be troublesome especially if the expanded or collapsed node includes a very big number of children processes that requires the user to go down or up in the graph looking for the node he just clicked on).
- [ ] Add a feature that allows drawing arrows from a node to another node located in another barnch (this is a practical example of process injection). For the time being, if the context includes an information about the process injection, you'll find the target process. Having the target process ID, you can search for that ID in the search bar and you'll find the target process you're looking for.
- [ ] Add a button that resets the Zoom to an initial state
- [ ] Add a feature that whitelists processes based on a search criteria (basically a command line or a regex of a command line) that will filter out any whitelisted process (and potentially its children as well).
- [ ] Mark the selected items from the search bar so that the user will not select them twice by mistake because he is thinking that item is interesting and it was not selected (and colored inside the graph before).

# How can I generate the [src/flare.json](src/flare.json) file from the raw logs?
If your logs are based on Falcon Crowdstrike, you can extract the event logs in .csv format and then use this tool [github.com/mohamedaymenkarmous/Falcon-Crowdstrike-Events-Processor](https://github.com/mohamedaymenkarmous/Falcon-Crowdstrike-Events-Processor) to process those logs. That tool will create the [src/flare.json](src/flare.json) file perfectly in the required format to make this EDR process explorer work.

# Support
This is an open source for any person who wants to contribute. Feel free to suggest any feature either verbally by creating a Github Issue or technically with a Pull Request.
