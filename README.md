<Architecture>

# app is deployed on EKS cluster in East2 Region.
# cluster contains two nopdes in EKS
# Create auto scaling groups for the two coantainerized apps 
# use Kubernetes Cluster Autoscaler or Karpenter to set up autoscaling of the nodes when the 2 initial nodes average 70%
# the VPC has two subnets in East 2 availability zone a and b

<db1.yml and db2.yml>

# This is the deployment definition file for the app1 and app2 database.
# Create EBS volume for DB Pod to enable persistent data<volume_ID>.
# Note: (a)volume must be tagged with the name of the cluster<my_cluster> for the pod to be able to attach to the volume.
#       (b) if there is already a data on the volume the pods will not be able to mount on the volume. Therefore pass an argument   to ignore the “lost and found” directory created when you formatted the volume.
# Pods should be running on the same zone where the EBS volume is created.
# Label nodes with Availability zone names to be used by the respective database pods for data persistent [Use node selector option which works with label]
# containerised MySQL image(base) from Dockerhub was used. 

<db_service>

# this is the service definition file for the database
# type is clusterIP as it is only acess from within the network
# assumed that the service will route requets on port 3306 to this pods

<secret.yml>

# used to encode the db password and refered in the environmental variables for the deployment

<app.yml>

# nodeselector can be used to restrict this app for avalability zone a and it's autoscaling group and same can be done for the second app in other to ensure independent scaling.
# App pods have multiple containers; conterner1: app image, container2: with an image<busybox> that will help to make sure that database pods are created first before the app pods.
# The image of this app is a tomcat based image asuming the countainer is exposed in port 8080
# this deployment is using a RollingUpdate stategy 

<app_service.yml>

# this is exposed to the web through loadBalancer listening at port 80
# traffics is then routed to port 8080 of the app pods and it's container






