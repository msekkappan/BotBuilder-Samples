﻿﻿This sample shows how to integrate LUIS to a bot with ASP.Net Core 2. 

# To try this sample
- Clone the samples repository
```bash
git clone https://github.com/Microsoft/botbuilder-samples.git
```
1. Install the Bot CLI Tools
```bash
npm install -g chatdown msbot ludown luis-apis qnamaker botdispatch luisgen
```

2. Setup Azure Powershell (If you have never done so before)
    - To login, run:
```bash
Connect-AzureRmAccount
```

    - To select your Azure subscription, run:
```bash
Select-AzureRmSubscription -Subscription "subscription-name"
```

3. Collect your Luis Authoring Key from the the [LUIS portal](https://www.luis.ai) by selecting your name in the top right corner. Save this key for the next step.

4. Run the following command from the project directory:
```bash
msbot clone services --name "<NAME>" --luisAuthoringKey "<YOUR AUTHORING KEY>" --folder "DeploymentScripts/MsbotClone" --location "westus" --verbose --appId <YOUR APP ID> --appSecret <YOUR APP SECRET PASSWORD>
```

**NOTE**: You can obtain your `appId` and `appSecret` at the Microsoft's [Application Registration Portal](https://apps.dev.microsoft.com/)

**NOTE**: By default your Luis Applications will be deployed to your free starter endpoint. An Azure LUIS service will be deployed along with your bot but you must manually add and publish to it from the luis.ai portal and update your key in the .bot file.

5. Note the generated secret generated by msbot. 
- The secret key is used later for `appsettings.json`.
```bash
The secret used to decrypt <NAME>.bot is:
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX=
NOTE: This secret is not recoverable and you should store this secret in a secure place according to best security practices.
Your project may be configured to rely on this secret and you should update it as appropriate.
```
5. Inspect Bot configuration file.
- The `msbot clone` command above generates a bot configuration file.
- The name of the bot configuration file is <NAME>.bot, where <NAME> is the name of your bot used in the `msbot clone` step.
- The configuration file can be loaded by the [Microsoft Bot Framework Emulator](https://aka.ms/botframeworkemulator).

6. Update <YOUR BOT CONFIGURATION> in `botbuilder-samples\samples\csharp_dotnetcore\21.luis-with-appsinsights\Startup.cs'.

```dotnet
        public void ConfigureServices(IServiceCollection services)
        {
            ...

            var botConfig = BotConfiguration.Load(botFilePath ??  @".\<YOUR BOT CONFIGURATION>.bot"..);
            ...
```

 7. Update "YOUR_LUIS_SERVICE_NAME" in `botbuilder-samples\samples\csharp_dotnetcore\21.luis-with-appsinsights\LuisBot.cs'.
 ```dotnet
     public class LuisBot : IBot
    {
       ...
        public static readonly string LuisKey = "YOUR_LUIS_SERVICE_NAME";
 ```
 8. Right click on the generated bot configuration file, click "Properties".
  - Ensure "Copy to Output Directory" is set to "Copy always".


 9. Update your `appsettings.json` with your .bot file path and .bot file secret.
- Update the `botFilePath` with the name of your generated bot configuration file.
- Update the `botFileSecret` with the secret that the msbot tool generated from step 5 above.

```bash
{
    "botFilePath": "<YOUR BOT CONFIG>.bot",
    "botFileSecret": "<YOUR BOT SECRET>"
}
```

## Prerequisites
### Set up LUIS
- Navigate to [LUIS portal](https://www.luis.ai).
- Click the `Sign in` button.
- Click on `My Apps`.
- Click on the `Import new app` button.
- Click on the `Choose File` and select [LUIS-Reminders.json](LUIS-Reminders.json) from the `botbuilder-samples\samples\csharp_dotnetcore\23.luis-with-appinsights\CognitiveModels` folder.
- Update [BotConfiguration.bot](BotConfiguration.bot) file with your AppId, SubscriptionKey, Region and Version. 
    You can find this information under "Publish" tab for your LUIS application at [LUIS portal](https://www.luis.ai).  For example, for
	https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/XXXXXXXXXXXXX?subscription-key=YYYYYYYYYYYY&verbose=true&timezoneOffset=0&q= 

    - AppId = XXXXXXXXXXXXX
    - SubscriptionKey = YYYYYYYYYYYY
    - Region =  westus

    The Version is listed on the page.
- Update [BotConfiguration.bot](BotConfiguration.bot) file with your Authoring Key.  
    You can find this under your user settings at [luis.ai](https://www.luis.ai).  Click on your name in the upper right hand corner of the portal, and click on the "Settings" menu option.
NOTE: Once you publish your app on LUIS portal for the first time, it takes some time for the endpoint to become available, about 5 minutes of wait should be sufficient.
### (Optional) Install LUDown
- (Optional) Install the LUDown [here](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUDown) to help describe language understanding components for your bot.
### Install Application Insights
  -  Follow instructions [here](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-asp-net-core) to set up your Application Insights service.
  - Note: The Application Insights will automatically update the [appsettings.json](appsettings.json) file.

# Running Locally

## Visual Studio
- Navigate to the samples folder (`botbuilder-samples\samples\csharp_dotnetcore\21.luis-with-appsinsights`) and open `LuisBotAppInsights.csproj` in Visual Studio 
- Run the project (press `F5` key).

## .NET Core CLI
- Install the [.NET Core CLI tools](https://docs.microsoft.com/en-us/dotnet/core/tools/?tabs=netcore2x). 
- Using the command line, navigate to `botbuilder-samples\samples\csharp_dotnetcore\21.luis-with-appsinsights` folder.
- Type 'dotnet run'.

## Testing the bot using Bot Framework Emulator
[Microsoft Bot Framework Emulator](https://aka.ms/botframeworkemulator) is a desktop application that allows bot developers to test and debug their bots on localhost or running remotely through a tunnel.

- Install the Bot Framework Emulator from [here](https://aka.ms/botframeworkemulator).

## Connect to bot using Bot Framework Emulator V4
- Launch the Bot Framework Emulator
- File -> Open bot and navigate to `botbuilder-samples\samples\csharp_dotnetcore\21.luis-with-appsinsights` folder.
- Select BotConfiguration.bot file.

# Deploy this bot to Azure
You can use the [MSBot](https://github.com/microsoft/botbuilder-tools) Bot Builder CLI tool to clone and configure any services this sample depends on. 

To install all Bot Builder tools - 

Ensure you have [Node.js](https://nodejs.org/) version 8.5 or higher

```bash
npm i -g msbot chatdown ludown qnamaker luis-apis botdispatch luisgen
```
To clone this bot, run
```
msbot clone services -f DeploymentScripts/MsbotClone -n <BOT-NAME> -l <Azure-location, ie westus> --subscriptionId <Azure-subscription-id>
```

# Further reading
- [Azure Bot Service](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0)
- [LUIS Documentation](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/)
- [Application Insights](https://azure.microsoft.com/en-us/services/application-insights/)
