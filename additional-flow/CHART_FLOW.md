# Data Modeling

### Pre-requisites
* node-red-contrib-mongodb4
* node-red-dashboard


This flow draws a chart using the data stored by the object-detection node.

### Enter start date and end date
![image](https://user-images.githubusercontent.com/78212696/194212333-2205002f-3c44-4dfd-8545-315e460352ea.png)

### Sum Chart
The values staying at the location within the period are displayed in the form of a bar chart.
![image](https://user-images.githubusercontent.com/78212696/194212470-5dd32923-158b-481f-9c66-c7375533c237.png)


### Pie Chart
The values staying at the location within the period are displayed in the form of a pie chart.
![image](https://user-images.githubusercontent.com/78212696/194212531-88ac9840-707e-4513-a109-673610d44016.png)

### Avg Chart
By averaging the values staying at the location within the period, the time spent at the location per day is displayed in the form of a bar chart.
![image](https://user-images.githubusercontent.com/78212696/194212556-70e4e4a0-7d14-4f5d-9fd6-c2a9470c7d06.png)

### input data format
```json
{ "label": "label"
, "payload": "data value"}
```
![image](https://user-images.githubusercontent.com/78212696/194212608-fdd645ff-600d-434f-8d33-182af1e3eaa4.png)

### Day Chart
The values staying at the location within the period are divided by date and displayed in the form of a bar chart.
![image](https://user-images.githubusercontent.com/78212696/194212701-98c0113b-9d00-4cf0-b2bd-8dc2a5d853ce.png)
![image](https://user-images.githubusercontent.com/78212696/194213705-2323cc46-91a3-4869-b23d-d23438fc3f45.png)

### Timeline Chart(Day)
The log value stored on the specified day is displayed as a timeline graph.
![image](https://user-images.githubusercontent.com/78212696/194213782-db6dae4b-8f3b-4b75-8bd4-72b72fb9aec9.png)
![image](https://user-images.githubusercontent.com/78212696/194213742-308ddebf-1576-40e2-b34e-e1f82e11b675.png)
```json
{"labels":["Bed","Couch"],"datasets":[{"label":"Bed","timestamp":"2022-09-27T14:29:26.395Z","data":[["2022-11-06T18:02:00.000Z","2022-11-06T18:22:00.000Z","#FF6633"],["2022-11-06T18:42:00.000Z","2022-11-06T19:01:00.000Z","#FF6633"]]},{"label":"Couch","timestamp":"2022-09-27T14:29:26.395Z","data":[["2022-11-06T18:22:00.000Z","2022-11-06T18:42:00.000Z","#5DA5DA"]]}]}
```

```json
{
	"label": "label",
	"datasets": {"label": "label",
		     "timestamp": "timestamp", 
		     "data": ["StartTime", "EndTime", "Color"]
		    }
}
```


