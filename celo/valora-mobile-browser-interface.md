# Introduction

This article is meant to show how you can write a small web app that you can use to connect to your mobile Valora wallet
You can use your laptop or mobile browser to connect to Valora wallet installed on your iPhone or Android phone.
This tutorial shows you how you can use @walletconnect/web3-provider and @celo/contractkit to read Account address, CELO and cUSD balances.

## Why do people use Valora

Valora is built on the Celo blockchain and it is used for fast, secured borderless payments. You can send money to a friend just by a simple click with near zero fees. More info here: https://valoraapp.com/

# Prerequisites

Please make sure you have a basic understanding about JavaScript based web development. The app uses React/react-native
Also you are familiar with crypto walets usage in general

# Requirements 

1. Install and configure Valora on your mobile phone
   Check out this tutorial: https://support.valoraapp.com/hc/en-us/articles/360061350131-Get-started-with-Valora

2. Make sure you have nvm installed. This helps you switch between node versions very easy, depending on project needs
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
```
3. Install and use node v17.3.0
```
nvm install v17.3.0
nvm use 17.3.0
```

# Project install & execution 

Clone the project (tested with node v17.3.0)
```
git clone https://github.com/mirceac/valora.git
```
The file package.json contains all libraries needed and all dependencies, so just run the following
```
cd valora
npm install
npm install --global expo-cli
```
Depending on the operating system where you need to start the application, you may need to run (from valora directory):
```
export NODE_OPTIONS=--openssl-legacy-provider
```
Start the application
```
expo start
```
![Test Image1](https://github.com/mirceac/valora/blob/master/images/startApp.png)

Opening your browser you will see the following screen

![Test Image2](https://github.com/mirceac/valora/blob/master/images/RunInBrowser.png)

Click on **Run in Web Browser** item on the left to launch the valora app

# Application workflow

The web app workflow is very simple. 
1. When page is loaded a "Connect" button is displayed.
 
   ```
   //Connect functionality: valora/App.js

     connect = async() => {
    	await provider.enable()
	const web3 = new Web3(provider);
        let kit = newKitFromWeb3(web3);
        kit.defaultAccount = provider.accounts[0]
        provider.on("accountsChanged", (accounts) => {
      	    console.log(accounts);
        });
        provider.on("connect", () => {
            console.log("connect");
        });
        const stableToken = await kit.contracts.getStableToken();
        const goldToken = await kit.contracts.getGoldToken();

        //const exchange = await kit.contracts.getExchange();

        const cUsdBalanceObj = await stableToken.balanceOf(kit.defaultAccount);
        const goldBalanceObj = await goldToken.balanceOf(kit.defaultAccount);
        const cUsdBalance = cUsdBalanceObj/10**18;
        const goldBalance = goldBalanceObj/10**18;
        this.setState({provider, kit, cUsdBalance, goldBalance});
   }
   
   ```
   ![Test Image 3](https://github.com/mirceac/valora/blob/master/images/valoraConnect.png)
   
2. When user clicks the button, a Wallet QR connection screen is displayed
   ![Test Image 4](https://github.com/mirceac/valora/blob/master/images/valoraQR.png)
   
3. Use the QR code to connect to Valora mobile wallet installed on your phone

4. Valora account address, Celo and cUSD balances are displayed.
   
   ![Test Image 5](https://github.com/mirceac/valora/blob/master/images/valoraData.png) 
   
   Clicking Back button will keep the connection in browser.
   Clicking Disconnect will close wallet connection
   
   ```
     //Disconnect functionality: valora/App.js
     disconnect = async() => {
       await provider.disconnect();
       this.setState({provider:null, kit: null});
     }
   ```
