To add functionalities for handling static IP plans similar to the existing Mikrotik class methods for hotspot and PPPoE plans, you need to create new methods within the Mikrotik class. These methods should correspond to the operations you want to perform for static IP plans, like adding, setting, or removing static IP configurations in the Mikrotik router.

Given that you already have methods for hotspot and PPPoE, you can use their structure as a template for your new static IP methods. However, the actual Mikrotik RouterOS commands you'll need to use may differ based on what you're trying to achieve with static IP plans.

Here's a basic outline of how you might structure these methods:

1. Add Static IP Plan
This method will add a new static IP plan to the Mikrotik router.

php
Copy code
public static function addStaticIpPlan($client, $name, $ipRange)
{
    global $_app_stage;
    if ($_app_stage == 'demo') {
        return null;
    }
    // Assuming RouterOS command for adding static IP plan is similar to the one below
    $addRequest = new RouterOS\Request('/ip/address/add');
    $client->sendSync(
        $addRequest
            ->setArgument('address', $ipRange)
            ->setArgument('comment', $name)
    );
}
2. Set Static IP Plan
This method updates an existing static IP plan.

php
Copy code
public static function setStaticIpPlan($client, $name, $ipRange)
{
    global $_app_stage;
    if ($_app_stage == 'demo') {
        return null;
    }
    // Fetch the ID of the existing IP plan
    $printRequest = new RouterOS\Request(
        '/ip/address print .proplist=.id',
        RouterOS\Query::where('comment', $name)
    );
    $planID = $client->sendSync($printRequest)->getProperty('.id');

    // Update the IP plan
    $setRequest = new RouterOS\Request('/ip/address/set');
    $client->sendSync(
        $setRequest
            ->setArgument('numbers', $planID)
            ->setArgument('address', $ipRange)
    );
}
3. Remove Static IP Plan
This method removes a static IP plan.

php
Copy code
public static function removeStaticIpPlan($client, $name)
{
    global $_app_stage;
    if ($_app_stage == 'demo') {
        return null;
    }
    $printRequest = new RouterOS\Request(
        '/ip/address print .proplist=.id',
        RouterOS\Query::where('comment', $name)
    );
    $planID = $client->sendSync($printRequest)->getProperty('.id');

    $removeRequest = new RouterOS\Request('/ip/address/remove');
    $client->sendSync(
        $removeRequest
            ->setArgument('numbers', $planID)
    );
}
Note: The actual RouterOS commands and arguments depend on what exactly you're trying to achieve with static IP plans. The commands used in the examples are hypothetical and should be replaced with the appropriate RouterOS API commands for managing static IP configurations. You should refer to the MikroTik RouterOS API documentation for the correct commands and parameters.

After defining these methods, you can call them in your application logic where you handle static IP plans, similar to how you currently handle hotspot and PPPoE plans. Remember to test these new methods thoroughly in a controlled environment before deploying them in a production environment.
