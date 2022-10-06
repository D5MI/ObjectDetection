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
