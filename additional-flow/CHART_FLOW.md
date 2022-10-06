# Data Modeling

### Pre-requisites

This flow draws a chart using the data stored by the object-detection node.

node-red-contrib-mongodb4

node-red-dashboard

### Enter start date and end date
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/407d4649-8dc4-4dea-bffc-35c73475e83d/Untitled.png)

### Sum Chart
The values staying at the location within the period are displayed in the form of a bar chart.
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/29a1bb59-a8c3-4f00-958b-21913e596801/Untitled.png)

### Pie Chart
The values staying at the location within the period are displayed in the form of a pie chart.
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/799f5ab8-7b5a-40fe-be12-b2e3d6d718b6/Untitled.png)

### Avg Chart
By averaging the values staying at the location within the period, the time spent at the location per day is displayed in the form of a bar chart.
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/35f548ef-bf5a-4bdd-b5d7-99ed0de46263/Untitled.png)

### input data format
```json
{ "label": "label"
, "payload": "data value"}
```
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b87553e3-fdaa-49c2-bf7d-37dcc75ed11d/Untitled.png)

### Day Chart
The values staying at the location within the period are divided by date and displayed in the form of a bar chart.
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/20118e4b-9c74-481e-9db7-085dee690918/Untitled.png)
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/86838534-e850-489e-9c9a-d8f5e65e4223/Untitled.png)

### Timeline Chart(Day)
The log value stored on the specified day is displayed as a timeline graph.
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8a1d0fcc-4f1f-4d40-8be5-fbf8cb4fee82/Untitled.png)
```json
{"labels":["Bed","Couch"],"datasets":[{"label":"Bed","timestamp":"2022-09-27T14:29:26.395Z","data":[["2022-11-06T18:02:00.000Z","2022-11-06T18:22:00.000Z","#FF6633"],["2022-11-06T18:42:00.000Z","2022-11-06T19:01:00.000Z","#FF6633"]]},{"label":"Couch","timestamp":"2022-09-27T14:29:26.395Z","data":[["2022-11-06T18:22:00.000Z","2022-11-06T18:42:00.000Z","#5DA5DA"]]}]}
```

```json
{
	'label": "label",
	"datasets": {"label": "label",
						 "timestamp": "timestamp", 
						 "data": ["StartTime", "EndTime", "Color"]
						}
}
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0360448c-6583-4eb7-ad6a-057b9a2baff2/Untitled.png)
