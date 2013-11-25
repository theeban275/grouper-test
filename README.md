# README

Setup grouper on a centos machine and run commands using the web service api.

### Requirements

* VirutalBox
* Vagrant

### Setup

After installing virtual box and vagrant.

```
# add vagrant box
vagrant box add CentOS_6.4_x86_64 http://developer.nrel.gov/downloads/vagrant-boxes/CentOS-6.4-x86_64-v20130731.box

# clone repository
git clone git@github.com:theeban275/grouper-test.git
cd grouper-test

# start vm and ssh into vm
vagrant up
vagrant ssh

# Download java sdk from here: http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html

# Install java sdk
rpm -Uvh jdk-7u45-linux-x64.rpm

# Install grouper
wget http://software.internet2.edu/grouper/release/2.1.5/grouper.installer-2.1.5.tar.gz
java -jar grouperInstaller.jar

# NOTE: Pay attention to the output if you need to restart grouper and the tomcat server

# Use default settings during installation
```

### Call API using WebService component
```
# Find groups using lite api (replace <password> with the GrouperSystem password entered during installation)
curl -v \
	-H "Content-Type: text/x-json" \
	--user GrouperSystem:<password> \
	-d '{ "WsRestFindGroupsLiteRequest": { "actAsSubjectId":"GrouperSystem", "groupName":"student", "queryFilterType":"FIND_BY_GROUP_NAME_APPROXIMATE" } }' \
	-X POST http://localhost:8080/grouper-ws/servicesRest/json/v2_1_5/groups 

# Find groups using normal api
curl -v \
	-H "Content-Type: text/x-json" \
	--user GrouperSystem:<password> \
	-d '{ "WsRestFindGroupsRequest": { "actAsSubjectLookup": { "subjectId":"GrouperSystem" }, "wsQueryFilter": { "groupName":"student", "queryFilterType":"FIND_BY_GROUP_NAME_APPROXIMATE" } } }' \
	-X POST http://localhost:8080/grouper-ws/servicesRest/json/v2_1_5/groups 

# Examples are provided here: http://anonsvn.internet2.edu/viewvc/viewvc.py/i2mi/trunk/grouper-ws/grouper-ws/doc/samples/findGroups/
```

### Useful links

* [Grouper Web Services](https://spaces.internet2.edu/display/Grouper/Grouper+Web+Services)
* [Grouper Installer](https://spaces.internet2.edu/display/Grouper/Grouper+Installer)
