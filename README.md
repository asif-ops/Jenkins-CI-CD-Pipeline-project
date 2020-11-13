# Create a Jenkins CI CD pipeline to deploy project website on AWS
## Problem statement:
Need to implement CI CD pipeline on jenkins as per below instructions –
* Create a jenkins master node and two slave nodes (slave 1 Test server) and (slave2 Production server) at AWS.
* Configure jenkins thus it Install the project website from git at slave-1 testing server on docker first, if successful then it should built the project website on slave2-production server. 
* Create pipeline view and Trigger the job using git web-hooks , whenever any change at git repository then jenkins should notify it and jenkins will automatically start build process.
