# Data Modeling

### ObjectDetection Result storage format (saved every second)

```json
{
  _id {
    $oid 63328e72995d727ec2d5171e
  },
  location Bed, 
  pose Stand,
  timestamp {
    $date {
      $numberLong 1665100800000
    }
  }
}
```

- location User's location detected by ObjectDetection
- pose User's pose detected by ObjectDetectio
- timestamp The time at which the detection result was saved in the log