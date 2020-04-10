## Objective :  
Label picture in local computer with AWS Rekognition service and Python 

## Prerequirement : 
- aws_access_key_id
- aws_secret_access_key
- Input image in jpg or png format 

## Python Script :
```
import boto3

client = boto3.client('rekognition',
                       aws_access_key_id = 'YourAccessKeyID',
                       aws_secret_access_key ='YourSecretAccessKey',
                       region_name = 'YourRegion')

with open('YourImage.jpg', 'rb') as source_image:
    source_bytes = source_image.read()

#You can specify the maximum number of labels and minimum confidence level as you want
#If MinConfidence is not specified,
#the operation returns labels with a confidence values greater than or equal to 55%
response = client.detect_labels(Image={'Bytes': source_bytes},MaxLabels=2, MinConfidence=90)

def label():
    for label in response['Labels']:
        print ("Label: " + label['Name'])
        print ("Confidence: " + str(label['Confidence']))
            
label()
```

<img src="https://user-images.githubusercontent.com/57988473/72610438-a43a1880-3927-11ea-8865-8408f5ba2289.png" width="900">


## Note :  
For more information about others available Amazon Rekognition methods :
https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rekognition.html
