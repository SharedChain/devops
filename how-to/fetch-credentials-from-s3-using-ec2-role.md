# Node.js - Fetch credentials securely from S3

## Install the AWS SDK

```
npm install aws-sdk
```

## Script to fetch the file

var AWS = require('aws-sdk');

var s3 = new AWS.S3();

var myBucket = '<bucket>';

var myKey = '<key>';

envFromS3(myBucket, myKey)

function envFromS3(bucket, key) {
  
  var params = {
    Bucket: bucket,
    Key: key
  };

  var s3file = s3.getObject(params).createReadStream();

  s3file
    .on('readable', function () {
      var obj;
      while (null !== (obj = s3file.read())) {
        try {
          var configs = JSON.parse(obj.toString('utf8'));

          var objectKeysArray = Object.keys(configs);

          objectKeysArray.forEach(function (objKey) {
            
            var objValue = configs[objKey];

            switch(typeof objValue) {
              case 'string':
              case 'boolean':
              case 'number':
                process.env[objKey] === objValue;
                console.log('Set process.env.' + objKey + ' to ' + objValue);
                break;
              default:
                break;
            }
          });
        } catch (err) {
          console.log(err);
        }
      }
    });
};

```