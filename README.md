

# Borndigital Poller

## Synopsis


The Born Digital Poller gets a record from Rabbit MQ and will try to add the data to the database.
If the data does not yet exist and has been added to the database, the Pid ID will be checked in Media Haven.
The status of the Essence is then checked; has it been archived on tape or disk, has it been deleted or has it failed?
If it is successfully archived on tape or disk, the Essence will be deleted and a message sent to Rabbit MQ.


## Technical

|Role              | Handle / username|
| -------------    |--------------| 
|Principal/Owner   | @VanCampJens | 
|Wing(wo)man       | @maartends |


## Stack

#### Backend
- Mule Community Edition ESB


## Logging and monitoring

#### Backend
- Logging: file


## Deployment/Installation

#### Prerequisites
- Mule Community Edition ESB
- Add the necessary values in `staging.yaml.example` and rename the file to `production`- or `staging.yaml` .

#### Backend

- Export .zip file from Anypoint Studio.
- cd to the folder where your .zip file is stored.
- Use a script to deploy app
*Example script:*

```bash
#!/bin/bash
ZIPFILE=$1
SERVER=$2
DEPLOYPATH="/opt/mule-community-standalone-3.9.0/apps"
USERNAME="root"

if [ ! -f $ZIPFILE ]; then
        echo "$ZIPFILE does not exist! Exiting"
        exit 1
fi

echo "Deploying Zipfile: $ZIPFILE to Server: $SERVER"

scp $ZIPFILE $USERNAME@$SERVER:$DEPLOYPATH
```


- To deploy use the command: `./nameOfScript.sh` (example: ./deployMuleApplication.sh).
- If you get the message: `Deploying Zipfile: nameOfZipFile.zip to Server: do-qas-esb-01.do.viaa.be`, you succesfully deployed the file.
- If you get the message: `nameOfZipFile.zip does not exist! Exiting`, something went wrong.
- Enter your password.



## Usage

#### Examples
The flow can be started via messages on the Borndigital Poller queue

### Troubleshooting

If, for some reason, a message does not get asked (and, thus, the poller-flow halts):

- undeploy application (remove anchor file)
- delete message (or purge queue)
- redeploy application (see "Deployment")
- publish message on queue.

