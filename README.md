# CareCrypt - Project

## Tech stack

    - Hyperledger Fabric blockchain (Node SDK JavaScript)
    - Node.js
    - Next.js
    - IPFS

<!-- ADD github access 

$ eval "$(ssh-agent -s)"
$ ssh-add ~/ssh/github -->

# Steps to setup project

## Download fabric binarys and fabric sample repo

    $ ./install-fabric.sh 

## To test network 

    $ cd fabric-samples/test-network
    $ ./network.sh up

    $ docker ps    // to check running container or check in docker desktop
    
    $ ./network.sh down     // to down network

## to run network with ca and create mychannel 

    $ cd fabric-samples/test-network
    
    Create network with ca cert: 
    
    $ ./network.sh up createChannel -ca -s couchdb
    
### Chain code deployment command

- Deploy chain code
	    
    $ ./network.sh deployCC -ccn careCryptChainCode -ccp ../asset-transfer-basic/chaincode-javascript/ -ccl javascript

    *Down Network - only if you want to stop network or close system
	
    $ ./network.sh down

### Register Admin

    $ cd server-node-sdk/
    $ node cert-script/registerOrg1Admin.js
    $ node cert-script/registerOrg2Admin.js

### onboard script
    
    $ node cert-script/onboardHospital01.js 
    $ node cert-script/onboardDoctor.js

    $ node cert-script/onboardInsuranceCompany.js 
    $ node cert-script/onboardInsuranceAgent.js

    *** you can use script to call chaincode and perform read and write opration on blockchain ledger. - optional *** 

### start node server to use api

    $ npm run dev

### API List
    
    1. registerPatient - as Patient
    2. loginPatient - as Patient
    3. grantAccess - to doctor from Patient
    4. addRecord - of Patient
    5. getRecordById - of Patient 
    6. getAllRecordByPatienId - filter record by patient
    7. fetchLedger - fetch all transaction only admin can fetch.

## chaincode logic

    - lets first understand the actors in our chaincode

    1. Goverment - network owner
    2. Hospital - Network orgination 
    3. Practicing physician / Doctor - member of hospital
    4. Diagnostics center - org OR peer of hospital
    5. Pharmacies - Org OR peer of hospital
    6. Researchers / R&D - org
    7. Insurance companies - org
    8. Patient - end user


   ## now lets see there read write access

        1. Goverment - network owner - admin access
        2. Hospital - Network orgination - Read/Write (doctor data)
        3. Practicing physician/Doctor - Read/Write (Patient data w.r.t to hospital)
        4. Diagnostics center - Read/Write (Patient records w.r.t to diagnostics center)
        5. Pharmacies - Read/Write (Patient prescriptions w.r.t to pharma center)
        6. Researchers / R&D - Read data of hospital conect, pateint based on consent. 
        7. Insurance companies - Read/Write (Patient claims)
        8. Patient - Read/Write (All generated patient data)

  ## object strucutre in db.

  [ "recordType"="hospital", "createdBy"="hospitalId", data={ name="ABC Hosptial", address="acb location"  } ]

  [ "recordType"="physician", "createdBy"="physicianID", data={ name="ABC Hosptial", address="acb location"  } ]

## Steps to setup explorer

Step 0:  $ cd fabric-explorer/

Step 1. Copy the orgination folder from your running network to explorer to get access of one of the node in network.

    $  sudo cp -r ../fabric-samples/test-network/organizations/ .
    $  cp -r ../fabric-samples/test-network/organizations/ .

Step 2. Export env vraiables.
    
    export EXPLORER_CONFIG_FILE_PATH=./config.json
    export EXPLORER_PROFILE_DIR_PATH=./connection-profile
    export FABRIC_CRYPTO_PATH=./organizations

 
Step 3. Edit test-network.json 
	
    Inside adminPrivateKey section check the path
    It should look like this (change the id which is present in your crypto certs)

Step 4: 
    to start with out logs

    $ docker-compose up -d                    

    to start with logs.
    
    $ docker-compose up

Step 5: To Stop Explorer
        $ docker-compose down
