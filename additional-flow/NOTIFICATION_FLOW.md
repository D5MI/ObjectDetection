# Notification_flow
### Pre-requisites
* node-red-contrib-samsung-automation-studio-nodes
* node-red-node-ui-list
* node-red-contrib-mongodb4
* node-red-dashboard

### Notification Setting Flow

Create an alarm by setting the location, pose, time, and device to be controlled and register it in MongoDB.
* You can select Object, Pose, Smart Things, running time.
* If you press button, the notification will be registered

![image](https://user-images.githubusercontent.com/72799490/194227165-0110c43c-8b58-498b-9b5c-1dea8d8db2c5.png)

### Notification call Flow

The log record stored in the database is called and when the time set in the notification comes, the user is notified through the device connected to SmartThings according to the conditions that match the option.

![image](https://user-images.githubusercontent.com/72799490/194227224-35cf8d55-baff-4799-9e83-0b6ca232a873.png)


<details>
<summary><h4>Notification_flow.json</h4></summary>

```json
[
    {
        "id": "61769f8b354516d0",
        "type": "inject",
        "z": "f81ca60cb491eedf",
        "name": "알람Flow",
        "props": [
            {
                "p": "time",
                "v": "",
                "vt": "date"
            }
        ],
        "repeat": "1",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "topic": "",
        "x": 310,
        "y": 700,
        "wires": [
            [
                "be3571a33fced222"
            ]
        ]
    },
    {
        "id": "b62f78e11f6e893d",
        "type": "ui_dropdown",
        "z": "f81ca60cb491eedf",
        "name": "",
        "label": "장소",
        "tooltip": "",
        "place": "Select option",
        "group": "233dcae6ee954e70",
        "order": 1,
        "width": 6,
        "height": 1,
        "passthru": false,
        "multiple": false,
        "options": [],
        "payload": "",
        "topic": "optionSetLocation",
        "topicType": "msg",
        "className": "",
        "x": 950,
        "y": 300,
        "wires": [
            [
                "cc94f3f6dedb7f8b"
            ]
        ]
    },
    {
        "id": "e40e2fd525e61af4",
        "type": "inject",
        "z": "f81ca60cb491eedf",
        "name": "",
        "props": [
            {
                "p": "payload"
            },
            {
                "p": "topic",
                "vt": "str"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": true,
        "onceDelay": 0.1,
        "topic": "",
        "payload": "",
        "payloadType": "date",
        "x": 450,
        "y": 360,
        "wires": [
            [
                "6db8b3e2572e16ce"
            ]
        ]
    },
    {
        "id": "6db8b3e2572e16ce",
        "type": "function",
        "z": "f81ca60cb491eedf",
        "name": "MongoDB Database Request For Bar Chart",
        "func": "\n\n        msg.payload = [\n            [{ $group: { \"_id\": \"$location\"} }]\n        ];\n        return msg;\n",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 410,
        "y": 300,
        "wires": [
            [
                "5163a08f78975d7d"
            ]
        ]
    },
    {
        "id": "5163a08f78975d7d",
        "type": "mongodb4",
        "z": "f81ca60cb491eedf",
        "clientNode": "e4c17237ec481b9e",
        "collection": "test3",
        "operation": "aggregate",
        "output": "toArray",
        "name": "",
        "x": 660,
        "y": 300,
        "wires": [
            [
                "d8e1ef2673ca1459"
            ]
        ]
    },
    {
        "id": "d8e1ef2673ca1459",
        "type": "function",
        "z": "f81ca60cb491eedf",
        "name": "parsing",
        "func": "var jsonfile = msg.payload;\nvar labelArray = new Array();\n\nvar label =\"\";\n\nvar resultArray = jsonfile.map(elm => ((elm._id)));\n\n\n\nmsg.options = resultArray;\nreturn msg;\n\n\n",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 800,
        "y": 300,
        "wires": [
            [
                "b62f78e11f6e893d"
            ]
        ]
    },
    {
        "id": "13f677558a820e4c",
        "type": "ui_button",
        "z": "f81ca60cb491eedf",
        "name": "",
        "group": "233dcae6ee954e70",
        "order": 5,
        "width": 6,
        "height": 1,
        "passthru": false,
        "label": "Submit",
        "tooltip": "",
        "color": "",
        "bgcolor": "",
        "className": "",
        "icon": "",
        "payload": "",
        "payloadType": "str",
        "topic": "go",
        "topicType": "str",
        "x": 940,
        "y": 180,
        "wires": [
            [
                "9ab40f04c3ccabcb"
            ]
        ]
    },
    {
        "id": "0c2be4a7050f8fe3",
        "type": "ui_dropdown",
        "z": "f81ca60cb491eedf",
        "name": "",
        "label": "포즈",
        "tooltip": "",
        "place": "Select option",
        "group": "233dcae6ee954e70",
        "order": 2,
        "width": 6,
        "height": 1,
        "passthru": false,
        "multiple": true,
        "options": [],
        "payload": "",
        "topic": "optionSetPose",
        "topicType": "msg",
        "className": "",
        "x": 950,
        "y": 260,
        "wires": [
            [
                "03bf5c2cfd1a6a5f"
            ]
        ]
    },
    {
        "id": "9ab40f04c3ccabcb",
        "type": "function",
        "z": "f81ca60cb491eedf",
        "name": "Store Data",
        "func": "var locationData = flow.get(\"locationData\") || 0; //get from context or default to 0\nvar timeData = flow.get(\"timeData\") || 0; //get from context or default to 0\nvar poseData = flow.get(\"poseData\") || [];\nvar deviceData = flow.get(\"deviceData\")|| 0;\n\nvar alarmDataArray = flow.get(\"alarmDataArray\") || [];\n\nswitch (msg.topic){\n\n    case \"optionSetTime\":\n        timeData = msg.payload;\n        if(timeData < 1){\n            node.warn(\"1분 이상으로 설정해 주세요!\");\n            return;\n        }\n           \n        flow.set(\"timeData\", timeData);//store in context for next time \n        return;\n        \n    case \"optionSetPose\":\n        poseData =msg.payload;\n        flow.set(\"poseData\", poseData); //store in context for next time \n        return;\n\n    case \"optionSetLocation\":\n        locationData = msg.payload;\n        flow.set(\"locationData\", locationData); //store in context for next time \n        return; \n\n    case \"optionSetDevice\":\n        deviceData = msg.payload;\n        flow.set(\"deviceData\", deviceData);\n        return;\n    case \"go\":\n    {\n            if (locationData === 0 || timeData === 0 || poseData === 0 || deviceData === 0){\n                node.warn(\"드롭 다운 데이터를 모두 입력해주세요!\");\n            }else{\n\n                msg.payload = [{ \"location\": locationData, \"setTime\": Number(timeData) * 60, \"poseData\": poseData, \"timestamp\": new Date(), \"deviceData\": deviceData}];\n\n\n            }\n\n    }\n\n      \n    \n}\n\nreturn msg;\n\n\n\n",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 1290,
        "y": 280,
        "wires": [
            [
                "17a25196ad918e9e"
            ]
        ]
    },
    {
        "id": "3f82db868256c0fd",
        "type": "change",
        "z": "f81ca60cb491eedf",
        "name": "",
        "rules": [
            {
                "t": "set",
                "p": "topic",
                "pt": "msg",
                "to": "optionSetTime",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 1110,
        "y": 220,
        "wires": [
            [
                "9ab40f04c3ccabcb"
            ]
        ]
    },
    {
        "id": "03bf5c2cfd1a6a5f",
        "type": "change",
        "z": "f81ca60cb491eedf",
        "name": "",
        "rules": [
            {
                "t": "set",
                "p": "topic",
                "pt": "msg",
                "to": "optionSetPose",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 1110,
        "y": 260,
        "wires": [
            [
                "9ab40f04c3ccabcb"
            ]
        ]
    },
    {
        "id": "cc94f3f6dedb7f8b",
        "type": "change",
        "z": "f81ca60cb491eedf",
        "name": "",
        "rules": [
            {
                "t": "set",
                "p": "topic",
                "pt": "msg",
                "to": "optionSetLocation",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 1110,
        "y": 300,
        "wires": [
            [
                "9ab40f04c3ccabcb"
            ]
        ]
    },
    {
        "id": "9ee27c965f06d4a3",
        "type": "ui_list",
        "z": "f81ca60cb491eedf",
        "group": "233dcae6ee954e70",
        "name": "",
        "order": 6,
        "width": 6,
        "height": 8,
        "lineType": "three",
        "actionType": "check",
        "allowHTML": true,
        "outputs": 1,
        "topic": "",
        "x": 1330,
        "y": 500,
        "wires": [
            [
                "43964845227f8522"
            ]
        ]
    },
    {
        "id": "43964845227f8522",
        "type": "function",
        "z": "f81ca60cb491eedf",
        "name": "function 20",
        "func": "var selectedObject = msg.payload;\n\nif (selectedObject.isChecked){\n    var selectedIdx = selectedObject._id;\n\n    msg.payload = [{ \"_id\": selectedIdx }];\n}\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 1330,
        "y": 540,
        "wires": [
            [
                "0d62df9b1b58c023"
            ]
        ]
    },
    {
        "id": "17a25196ad918e9e",
        "type": "mongodb4",
        "z": "f81ca60cb491eedf",
        "clientNode": "e5eea5fa320c8736",
        "collection": "optionSave",
        "operation": "insert",
        "output": "toArray",
        "name": "",
        "x": 1450,
        "y": 280,
        "wires": [
            [
                "f5f08f431c515551"
            ]
        ]
    },
    {
        "id": "0d62df9b1b58c023",
        "type": "mongodb4",
        "z": "f81ca60cb491eedf",
        "clientNode": "e5eea5fa320c8736",
        "collection": "optionSave",
        "operation": "remove",
        "output": "toArray",
        "name": "",
        "x": 1540,
        "y": 460,
        "wires": [
            [
                "f5f08f431c515551"
            ]
        ]
    },
    {
        "id": "7c7845482ee591f7",
        "type": "inject",
        "z": "f81ca60cb491eedf",
        "name": "",
        "props": [
            {
                "p": "payload"
            },
            {
                "p": "topic",
                "vt": "str"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": true,
        "onceDelay": 0.1,
        "topic": "",
        "payload": "",
        "payloadType": "date",
        "x": 470,
        "y": 200,
        "wires": [
            [
                "5dbbc13bb7644c79"
            ]
        ]
    },
    {
        "id": "5dbbc13bb7644c79",
        "type": "function",
        "z": "f81ca60cb491eedf",
        "name": "MongoDB Database Request For Bar Chart",
        "func": "\n\n        msg.payload = [\n            [{ $group: { \"_id\": \"$pose\"} }]\n        ];\n        return msg;\n",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 410,
        "y": 260,
        "wires": [
            [
                "7fabe84bdbb87fc9"
            ]
        ]
    },
    {
        "id": "7fabe84bdbb87fc9",
        "type": "mongodb4",
        "z": "f81ca60cb491eedf",
        "clientNode": "e4c17237ec481b9e",
        "collection": "test3",
        "operation": "aggregate",
        "output": "toArray",
        "name": "",
        "x": 660,
        "y": 260,
        "wires": [
            [
                "9103b91432ff4133"
            ]
        ]
    },
    {
        "id": "9103b91432ff4133",
        "type": "function",
        "z": "f81ca60cb491eedf",
        "name": "parsing",
        "func": "var jsonfile = msg.payload;\nvar labelArray = new Array();\n\nvar label =\"\";\n\nvar resultArray = jsonfile.map(elm => ((elm._id)));\n\n\n\nmsg.options = resultArray;\nreturn msg;\n\n\n",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 800,
        "y": 260,
        "wires": [
            [
                "0c2be4a7050f8fe3"
            ]
        ]
    },
    {
        "id": "8240322e0d6c8f9c",
        "type": "ui_text_input",
        "z": "f81ca60cb491eedf",
        "name": "",
        "label": "분",
        "tooltip": "",
        "group": "233dcae6ee954e70",
        "order": 4,
        "width": 0,
        "height": 0,
        "passthru": true,
        "mode": "number",
        "delay": "0",
        "topic": "topic",
        "sendOnBlur": true,
        "className": "",
        "topicType": "msg",
        "x": 950,
        "y": 220,
        "wires": [
            [
                "3f82db868256c0fd"
            ]
        ]
    },
    {
        "id": "f5f08f431c515551",
        "type": "function",
        "z": "f81ca60cb491eedf",
        "name": "findAll",
        "func": "\nmsg.payload=[{}];\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 1470,
        "y": 360,
        "wires": [
            [
                "c2d30e5c6961712b"
            ]
        ]
    },
    {
        "id": "c2d30e5c6961712b",
        "type": "mongodb4",
        "z": "f81ca60cb491eedf",
        "clientNode": "e5eea5fa320c8736",
        "collection": "optionSave",
        "operation": "find",
        "output": "toArray",
        "name": "",
        "x": 1610,
        "y": 360,
        "wires": [
            [
                "9218fe3cdd7a9190"
            ]
        ]
    },
    {
        "id": "9218fe3cdd7a9190",
        "type": "function",
        "z": "f81ca60cb491eedf",
        "name": "function 21",
        "func": "var alarmDataArray = msg.payload;\nvar alarmHtmlArray = [];\n\nfor (var i = 0; i < alarmDataArray.length;i++){\n    var locationDataHtml = \"<b>\" + alarmDataArray[i].location + \"</b>\";\n    var description = alarmDataArray[i].setTime / 60 + \" 분 동안 \" + alarmDataArray[i].poseData + \" 동작을 할때 알림 \\n\" + \"동작 기기 \"+alarmDataArray[i].deviceData;\n    alarmHtmlArray.push({ \"title\": locationDataHtml, \"description\": description, \"_id\": alarmDataArray[i]._id, \"isChecked\": false});\n    }\n\nmsg.payload = alarmHtmlArray;\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 1330,
        "y": 460,
        "wires": [
            [
                "9ee27c965f06d4a3"
            ]
        ]
    },
    {
        "id": "68a37468861af23f",
        "type": "inject",
        "z": "f81ca60cb491eedf",
        "name": "",
        "props": [
            {
                "p": "payload"
            },
            {
                "p": "topic",
                "vt": "str"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": true,
        "onceDelay": 0.1,
        "topic": "",
        "payload": "",
        "payloadType": "date",
        "x": 1270,
        "y": 360,
        "wires": [
            [
                "f5f08f431c515551"
            ]
        ]
    },
    {
        "id": "be3571a33fced222",
        "type": "function",
        "z": "f81ca60cb491eedf",
        "name": "findAll",
        "func": "msg.payload=[{}];\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 450,
        "y": 700,
        "wires": [
            [
                "48fa44773719e1ad"
            ]
        ]
    },
    {
        "id": "48fa44773719e1ad",
        "type": "mongodb4",
        "z": "f81ca60cb491eedf",
        "clientNode": "e5eea5fa320c8736",
        "collection": "optionSave",
        "operation": "find",
        "output": "toArray",
        "name": "",
        "x": 570,
        "y": 700,
        "wires": [
            [
                "0d8b5c97ac484a33"
            ]
        ]
    },
    {
        "id": "0d8b5c97ac484a33",
        "type": "split",
        "z": "f81ca60cb491eedf",
        "name": "",
        "splt": "\\n",
        "spltType": "str",
        "arraySplt": 1,
        "arraySpltType": "len",
        "stream": false,
        "addname": "",
        "x": 710,
        "y": 700,
        "wires": [
            [
                "137045f2d05fbfb9"
            ]
        ]
    },
    {
        "id": "137045f2d05fbfb9",
        "type": "function",
        "z": "f81ca60cb491eedf",
        "name": "function 29",
        "func": "var t = new Date(msg.time);\n\nvar alarmData = msg.payload ;\nvar startDate = alarmData.timestamp;\nstartDate = new Date(startDate);\n\n\n\nt = new Date();\n////\n\nt.setMilliseconds(0);\n\n\n\nmsg.payload = [\n    [\n    \n        { $match: { \"location\": alarmData.location ,\"timestamp\": { $gte: new Date(startDate), $lte: t },\"pose\": { \"$in\": alarmData.poseData}}},\n        { $group: { _id: alarmData._id, payload: { $sum: 1 } } }\n    ]\n];\n\nmsg.data =alarmData;\n\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 850,
        "y": 700,
        "wires": [
            [
                "c111de47c49e5145"
            ]
        ]
    },
    {
        "id": "c111de47c49e5145",
        "type": "mongodb4",
        "z": "f81ca60cb491eedf",
        "clientNode": "e4c17237ec481b9e",
        "collection": "test3",
        "operation": "aggregate",
        "output": "forEach",
        "name": "",
        "x": 1020,
        "y": 700,
        "wires": [
            [
                "3c2fedaf23f7700e"
            ]
        ]
    },
    {
        "id": "3c2fedaf23f7700e",
        "type": "function",
        "z": "f81ca60cb491eedf",
        "name": "function 30",
        "func": "//aggregation을 통해 가져온 데이터\nvar sumData = msg.payload;\n\n//현재 입력되어있는 옵션 데이터\nvar curData = msg.data;\n\n//알람 울리는 조건 만족\nif(sumData.payload >= curData.setTime){\n\n    msg.alarm = curData.deviceData;\n    msg.payload = [{ \"_id\": curData._id }, { \"$set\": { timestamp: new Date() }}];\n    return msg;\n}\n",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 1190,
        "y": 700,
        "wires": [
            [
                "ac865ed73016e0c2",
                "84adecf89d4f6803"
            ]
        ]
    },
    {
        "id": "df864e103623bc71",
        "type": "switch",
        "z": "f81ca60cb491eedf",
        "name": "",
        "property": "payload",
        "propertyType": "msg",
        "rules": [
            {
                "t": "eq",
                "v": "가상스위치",
                "vt": "str"
            },
            {
                "t": "eq",
                "v": "가상스위치2",
                "vt": "str"
            },
            {
                "t": "eq",
                "v": "전구",
                "vt": "str"
            }
        ],
        "checkall": "true",
        "repair": false,
        "outputs": 3,
        "x": 1690,
        "y": 700,
        "wires": [
            [],
            [],
            []
        ]
    },
    {
        "id": "ac865ed73016e0c2",
        "type": "mongodb4",
        "z": "f81ca60cb491eedf",
        "clientNode": "e4c17237ec481b9e",
        "collection": "optionSave",
        "operation": "update",
        "output": "forEach",
        "name": "",
        "x": 1190,
        "y": 780,
        "wires": [
            []
        ]
    },
    {
        "id": "d2fad0547015f6d3",
        "type": "ui_dropdown",
        "z": "f81ca60cb491eedf",
        "name": "",
        "label": "기기",
        "tooltip": "",
        "place": "Select option",
        "group": "233dcae6ee954e70",
        "order": 3,
        "width": 0,
        "height": 0,
        "passthru": false,
        "multiple": true,
        "options": [
            {
                "label": "가상스위치",
                "value": "가상스위치",
                "type": "str"
            },
            {
                "label": "가상스위치2",
                "value": "가상스위치2",
                "type": "str"
            },
            {
                "label": "전구",
                "value": "전구",
                "type": "str"
            }
        ],
        "payload": "",
        "topic": "topic",
        "topicType": "msg",
        "className": "",
        "x": 910,
        "y": 400,
        "wires": [
            [
                "030087f07e012030"
            ]
        ]
    },
    {
        "id": "030087f07e012030",
        "type": "change",
        "z": "f81ca60cb491eedf",
        "name": "",
        "rules": [
            {
                "t": "set",
                "p": "topic",
                "pt": "msg",
                "to": "optionSetDevice",
                "tot": "str"
            }
        ],
        "action": "",
        "property": "",
        "from": "",
        "to": "",
        "reg": false,
        "x": 1070,
        "y": 400,
        "wires": [
            [
                "9ab40f04c3ccabcb"
            ]
        ]
    },
    {
        "id": "55f03bb28bceb677",
        "type": "split",
        "z": "f81ca60cb491eedf",
        "name": "",
        "splt": "\\n",
        "spltType": "str",
        "arraySplt": 1,
        "arraySpltType": "len",
        "stream": false,
        "addname": "",
        "x": 1550,
        "y": 700,
        "wires": [
            [
                "df864e103623bc71"
            ]
        ]
    },
    {
        "id": "84adecf89d4f6803",
        "type": "function",
        "z": "f81ca60cb491eedf",
        "name": "function 31",
        "func": "\nmsg.payload = msg.alarm\nreturn msg;",
        "outputs": 1,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 1390,
        "y": 700,
        "wires": [
            [
                "55f03bb28bceb677"
            ]
        ]
    },
    {
        "id": "233dcae6ee954e70",
        "type": "ui_group",
        "name": "Alarm",
        "tab": "deebf5652e8a622b",
        "order": 4,
        "disp": true,
        "width": "6",
        "collapse": false,
        "className": ""
    },
    {
        "id": "e4c17237ec481b9e",
        "type": "mongodb4-client",
        "name": "AWS mongoDB",
        "protocol": "mongodb",
        "hostname": "J7S005.p.ssafy.io",
        "port": "",
        "dbName": "flow_db",
        "authSource": "",
        "authMechanism": "DEFAULT",
        "tls": false,
        "tlsCAFile": "",
        "tlsInsecure": false,
        "uri": "",
        "advanced": "",
        "uriTabActive": "tab-uri-simple"
    },
    {
        "id": "e5eea5fa320c8736",
        "type": "mongodb4-client",
        "name": "",
        "protocol": "mongodb",
        "hostname": "J7S005.p.ssafy.io",
        "port": "",
        "dbName": "flow_db",
        "authSource": "",
        "authMechanism": "DEFAULT",
        "tls": false,
        "tlsCAFile": "",
        "tlsInsecure": false,
        "uri": "",
        "advanced": "",
        "uriTabActive": "tab-uri-simple"
    },
    {
        "id": "deebf5652e8a622b",
        "type": "ui_tab",
        "name": "Alarm",
        "icon": "dashboard",
        "disabled": false,
        "hidden": false
    }
]
```


</details>

