# composer-meetup

Step Four: Adding a Model File
The first step is to add a Model file. Model files define the assets, participants, transactions, and events in
our business network.
1. Click the Add a file button.
2. Click the Model file and click Add.
3. Delete the lines of code in the model file and replace it with this:


/**
* My commodity trading network
*/
namespace org.acme.mynetwork
asset Commodity identified by tradingSymbol {
o String tradingSymbol
o String description
o String mainExchange
o Double quantity
--> Trader owner
}
participant Trader identified by tradeId {
o String tradeId
o String firstName
o String lastName
}
transaction Trade {
--> Commodity commodity
--> Trader newOwner
}



Step Five: Adding a Transaction Processor Script file
1. Click the Add a file button.
2. Click the Script file and click Add.
3. Delete the lines of code in the script file and replace it with the following code:


/**
* Track the trade of a commodity from one trader to another
* @param {org.acme.mynetwork.Trade} trade - the trade to be processed
* @transaction
*/
function tradeCommodity(trade) {
trade.commodity.owner = trade.newOwner;
return getAssetRegistry('org.acme.mynetwork.Commodity')
.then(function (assetRegistry) {
return assetRegistry.update(trade.commodity);
});
}


Step Six: Adding an Access Control File
1. Click the Add a file button
2. Click the Access Control file and click Add
3. Delete the lines of code in the access control file and replace it with the following code:

/** * Access control rules for mynetwork */
rule Default {
description: "Allow all participants access to all resources"
participant: "ANY" operation: ALL resource: "org.acme.mynetwork.*"
action: ALLOW
}
rule SystemACL {
description: "System ACL to permit all access"
participant: "org.hyperledger.composer.system.Participant"
operation: ALL
resource: "org.hyperledger.composer.system.**"
action: ALLOW
}

Step Nine: Creating Participants
1. Ensure that you have the Trader tab selected on the left, and click Create New Participant in the
upper right.
2. What you can see is the data structure of a Trader participant. We want some easily recognizable
data, so delete the code that's there and paste the following:


{
"$class": "org.acme.mynetwork.Trader",
"tradeId": "TRADER1",
"firstName": "Jenny",
"lastName": "Jones"
}
3. Click Create New to create the participant.
4. You should be able to see the new Trader participant you've created. We need another Trader to
test our Trade transaction though, so create another Trader, but this time, use the following data:
{
"$class": "org.acme.mynetwork.Trader",
"tradeId": "TRADER2",
"firstName": "Amy",
"lastName": "Williams"
}


Step Ten: Creating an Asset
1. Click the Commodity tab under Assets and click Create New Asset.
2. Delete the asset data and replace it with the following:
{
"$class": "org.acme.mynetwork.Commodity",
"tradingSymbol": "ABC",
"description": "Test commodity",
"mainExchange": "Euronext",
"quantity": 72.297,
"owner":
"resource:org.acme.mynetwork.Trader#TRADER1"
}



Step Eleven: Transferring the Commodity
To test the Trade transaction:
▪ Click the Submit Transaction button on the left
▪ Ensure that the transaction type is Trade
▪ Replace the transaction data with the following:
{
"$class": "org.acme.mynetwork.Trade",
"commodity":
"resource:org.acme.mynetwork.Commodity#ABC",
"newOwner":
"resource:org.acme.mynetwork.Trader#TRADER2"
}


