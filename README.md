COMPOSER_CARD=restadmin@property-network
COMPOSER_NAMESPACES=never
COMPOSER_AUTHENTICATION=true
COMPOSER_MULTIUSER=true
COMPOSER_PROVIDERS='{
"google": {
    "provider": "google",
    "module": "passport-google-oauth2",
    "clientID": "614221525051-un957n5hg67a62c19mfd09opgmsd0sc4.apps.googleusercontent.com",
        "clientSecret": "pBoU6TOCVpLon22ugg8_-crb",
        "authPath": "/auth/google",
        "callbackURL": "/auth/google/callback",
        "scope": "https://www.googleapis.com/auth/plus.login",
        "successRedirect": "/",
        "failureRedirect": "/"
  }
}'
COMPOSER_DATASOURCES='{
    "db": {
        "name": "db",
        "connector": "mongodb",
        "host": "mongo"
    }
}'

composer network start --card PeerAdmin@hlfv1 --networkName one-network --networkVersion 0.1.1-deploy.2 --networkAdmin admin --networkAdminEnrollSecret adminpw --file networkadmin.card
//Run the following

'{"$class:org.acme.model.NetworkAdmin#admin","personId":"leowyennhan@gmail.com","firstName":"Leow","lastName":"YennHan"}'

composer archive create -t dir -n .
composer runtime install --card PeerAdmin@hlfv1 --businessNetworkName property-network

composer network start --card PeerAdmin@hlfv1 --networkName one-network --networkVersion 0.1.1-deploy.11 --networkAdmin admin --networkAdminEnrollSecret adminpw --file networkadmin.card


composer network start --card PeerAdmin@hlfv1 --networkAdmin admin --networkAdminEnrollSecret adminpw --archiveFile property-network.bna --file networkadmin.card
composer card import --file networkadmin.card
composer network ping --card admin@property-network

composer-rest-server -c admin@property-network -a true

After some research, I found a solution and worked for me. If you already enable Github authentication then ignore. Otherwise first enable authentication following this tutorial Enaling Authentication.
Before start rest server you will export your admin card from the network by using this command:

//start with no authentication  
 composer-rest-server -c admin@property-network -n never -w true

    
composer card export -n admin@property-network -f networkadmin.card
Now start rest server with authentication using this command:
composer-rest-server -c admin@property-network -p 3000 -a true -m true
After some time rest server will start. Now First, go this link for authentication: http://localhost:3000/auth/github

After successful authentication, you will get an access token and also you will see a Wallet options below. Now you need to import a card that you already export from your network. That's it, you can able to add anything to your network.
The BNA is the Business Network Archive (an archive/zip file containing the definition of the Business Network.) When this is running you can create Participants and Issue IDs to Participants which result in a .card file (which is also an acchive/zip). You can then Import the .Card file into the Wallet in the REST server. If you use MongoDB as a Datasource then the Wallets will be persisted in Mongo and will be available next time you start the REST server.

So as mentioned 
1) create yourself a participant in your BN called 'restadmin' 
2) from the CLI do `composer identity issue -c admin@property-network -u restadmin -a "resource:org.hyperledger.composer.system.NetworkAdmin#admin" -x  -f rest.card ` and please do change the fully qualified -a in your command line for restadmin <wherever you create the participant in your own custom business network namespace> then simply go to your REST API 
3) From REST APIs (Under /System;) issue the identity for 'restadmin' (format of JSON string to provide is shown) and 4) do a /POST /wallet/import and import the card (file..browse...explore..file) find `rest.card` in the filesystem then -> as shown here -> https://stackoverflow.com/questions/48838803/how-to-create-participant-there-identities-via-rest-api-that-generated-by-comp



Google oauth
client ID - 614221525051-un957n5hg67a62c19mfd09opgmsd0sc4.apps.googleusercontent.com
client secret - pBoU6TOCVpLon22ugg8_-crb

sed -e 's/localhost:/orderer.example.com:/' -e 's/localhost:/peer0.org1.example.com:/' -e 's/localhost:/peer0.org1.example.com:/' -e 's/localhost:/ca.org1.example.com:/'  < $HOME/.composer/cards/restadmin@property-network/connection.json  > /tmp/connection.json && cp -p /tmp/connection.json $HOME/.composer/cards/restadmin@property-network/


docker run \
    -d \
    -e COMPOSER_CARD=${COMPOSER_CARD} \
    -e COMPOSER_NAMESPACES=${COMPOSER_NAMESPACES} \
    -e COMPOSER_AUTHENTICATION=${COMPOSER_AUTHENTICATION} \
    -e COMPOSER_MULTIUSER=${COMPOSER_MULTIUSER} \
    -e COMPOSER_PROVIDERS="${COMPOSER_PROVIDERS}" \
    -e COMPOSER_DATASOURCES="${COMPOSER_DATASOURCES}" \
    -v ~/.composer:/home/composer/.composer \
    --name rest \
    --network composer_default \
    -p 3000:3000 \
    myorg/composer-rest-server

        composer participant add -c admin@one-network -d '{"$class":"org.hyperledger.composer.system.NetworkAdmin", "participantId":"restadmin"}'  
    composer identity issue -c admin@one-network -f restadmin.card -u restadmin -a "resource:org.hyperledger.composer.system.NetworkAdmin#restadmin"
    composer card import -f restadmin.card
    composer network ping -c restadmin@one-network

    sed -e 's/localhost:/orderer.example.com:/' -e 's/localhost:/peer0.org1.example.com:/' -e 's/localhost:/peer0.org1.example.com:/' -e 's/localhost:/ca.org1.example.com:/'  < $HOME/.composer/cards/restadmin@one-network/connection.json  > /tmp/connection.json && cp -p /tmp/connection.json $HOME/.composer/cards/restadmin@one-network/

composer participant add -c admin@one-network -d '{"$class":"org.acme.model.owner","ownerID":"owner0","nric":"owner_01_931111232314", "firstName":"mike","lastName":"Leow", "email_address":"leowyennhan@gmail.com", "type_of_employee":"self_employed","income_amount":18888888,"ccris_score":5,"ctos_score":5}'


    docker run -d --name mongo --network composer_default -p 27017:27017 mongo
    property-network
    composer network start -c PeerAdmin@hlfv1 -A admin -S adminpw -a property-network.bna -f networkadmin.card  

    composer participant add -c admin@one-network -d '{"$class":"org.acme.model.owner","ownerID":"owner1","nric":"owner_1_960122106187", "firstName":"Mike","lastName":"Leow", "email_address":"leowyennhan@gmail.com", "type_of_employee":"self_employed","income_amount":168888888,"ccris_score":3,"ctos_score":2}'

    composer identity issue -c admin@one-network -f mikeleow.card -u mikeleow -a "resource:org.acme.model.owner#owner0"

    composer card import -f mikeleow.card   

    sed -e 's/localhost:/orderer.example.com:/' -e 's/localhost:/peer0.org1.example.com:/' -e 's/localhost:/peer0.org1.example.com:/' -e 's/localhost:/ca.org1.example.com:/' < $HOME/.composer/cards/mikeleow@one-network/connection.json > /tmp/connection.json && cp -p /tmp/connection.json $HOME/.composer/cards/mikeleow@one-network

    composer card export -f mikeleow_exp.card -c mikeleow@one-network ; rm mikeleow.card

    composer card export -f restadmin1.card -c restadmin@one-network ; rm restadmin.card


    npm install -g composer-cli@0.17.6
    npm install -g composer-rest-server@0.17.6
    npm install -g generator-hyperledger-composer@0.17.6
    npm install -g yo
    npm install -g composer-playground@0.17.6
    

     FROM hyperledger/composer-rest-server:17.6
      
     RUN npm install --production loopback-connector-mongodb passport-google-oauth2 && \
     npm cache clean --force && \
     ln -s node_modules .node_modules

     composer participant add -c admin@property-network -d '{"$class":"org.hyperledger.composer.system.NetworkAdmin", "participantId":"leow"}'
     composer identity issue -c admin@property-network -f leow.card -u leow -a "resource:org.hyperledger.composer.system.NetworkAdmin#leow"
    composer card import -f leow.card
    composer network ping -c leow@property-network

    sed -e 's/localhost:/orderer.example.com:/' -e 's/localhost:/peer0.org1.example.com:/' -e 's/localhost:/peer0.org1.example.com:/' -e 's/localhost:/ca.org1.example.com:/' < $HOME/.composer/cards/leow@property-network/connection.json > /tmp/connection.json && cp -p /tmp/connection.json $HOME/.composer/cards/leow@property-network

    composer card export -f leow1.card -n leow@property-network ; rm leow.card


    composer participant add -c admin@property-network -d '{"$class":"org.acme.model.owner","ownerID":"owner02", "firstName":"Dexter","lastName":"Leow", "email_address":"dexterleow93@gmail.com", "type_of_employeee":"self_employed"}'

    composer identity issue -c admin@property-network -f Dleow.card -u Dleow -a "resource:org.acme.model.owner#owner02"
    composer card import -f Dleow.card
    sed -e 's/localhost:/orderer.example.com:/' -e 's/localhost:/peer0.org1.example.com:/' -e 's/localhost:/peer0.org1.example.com:/' -e 's/localhost:/ca.org1.example.com:/' < $HOME/.composer/cards/Dleow@property-network/connection.json > /tmp/connection.json && cp -p /tmp/connection.json $HOME/.composer/cards/Dleow@property-network

    composer card export -f Dleow1.card -n Dleow@property-network ; rm Dleow.card

    composer participant add -c admin@One-Identity-Network---25 -d '{"$class":"org.acme.model.owner","ownerID":"owner02", "firstName":"Dexter","lastName":"Leow", "email_address":"dexterleow93@gmail.com", "type_of_employeee":"self_employed"}'


5a09a7c76c
composer card create -f ca.card -p connection-profile.json -u admin -s ${SECRET}

composer card create -f adminCard.card -p ./connection.json -u admin -c ./credentials/admin-pub.pem -k ./credentials/admin-priv.pem --role PeerAdmin --role ChannelAdmin

composer card create -f adminCard.card -p connection.json -u admin -c ./credentials/admin-pub.pem -k ./credentials/admin-priv.pem --role PeerAdmin --role ChannelAdmin

 composer card create -f adminCard.card -p creds.json -u admin -c ./credentials/admin-pub.pem -k ./credentials/admin-priv.pem --role PeerAdmin --role ChannelAdmin

  composer identity import -p bmx-hlfv1 -u admin -c ~/.identityCredentials/admin-pub.pem -k ~/.identityCredentials/admin-priv.pem

composer network start -c adminCard -a property-network.bna -A admin -C ./credentials/admin-pub.pem -f delete_me.card

 composer network start -c adminCard -a property-network.bna -A admin -C ./credentials/admin-pub.pem -f delete_me.card
cat /Users/leowyennhan/Desktop/credentials/admin-pub.pem

export NODE_CONFIG='{"composer":{"wallet":{"type":"composer-wallet-s3","options":{"bucketName":"one-identity-network"}}}}'


