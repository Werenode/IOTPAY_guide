
<details>
  <summary> Getting Started Locally </summary>

#### To run the project locally, make sure you have the following tools installed on your system:

Prerequisites:
	
-   **Node.js v18**: You can download it from the official Node.js website.

-	**Docker**: Make sure Docker is installed and running. You can get it here.

-	**MetaMask** browser extension: Required to interact with the app using a Web3 wallet. Install it from the MetaMask website.

- **Angular** CLI v17.0.1: This project was generated using Angular CLI.


# Project Setup

Follow these steps to set up and run all parts of the system locally:


## 1. Backend (Server)

Clone and run the backend server:

```bash
git clone https://github.com/Werenode/IOTPAY_server
cd IOTPAY_server
npm install
npm run docker_start   # Starts required Docker services
npm run start          # Starts the backend server
```

## 2. Dashboard (Frontend)

In a new terminal window, clone and run the dashboard interface:

```bash
git clone https://github.com/Werenode/dashboard_for_iotpay
cd dashboard_for_iotpay
npm install
npm run start
```

This will launch the Angular-based dashboard in development mode. By default, it should be available at http://localhost:4200.


## 3. Simulated Client (MQTT Device)

To simulate a connected IoT device over MQTT:

```bash
git clone https://github.com/Werenode/IOTPAY_client_mqtt
cd IOTPAY_client_mqtt
npm install
npm run start
```

This will start a client that communicates with the backend via MQTT protocol.

</details>


# Using the Dashboard

The Dashboard provides a visual interface to manage fleets, services, and IoT devices through your Web3 wallet. Below is a step-by-step guide to configure and operate your system.

### About the “Delegated” Option in Forms

Across most forms in the Dashboard, you’ll notice a checkbox labeled “Delegated”.
This option determines how a transaction will be sent to the blockchain:

___

If “Delegated” is not checked:

-	The transaction will be sent directly from your wallet using MetaMask.

-	You will see a MetaMask popup requesting you to confirm and broadcast the transaction.

-	You’ll need to pay the gas fees yourself.

___

If “Delegated” is checked:

-	You will sign the transaction data using your wallet, but you won’t send it yourself.

-	The dashboard will have fields for how long your signature should remain valid (default is 5 minutes).

-	The server will send the transaction on your behalf during that time.


**Note**: The smart contract will verify your signature to ensure you authorized the action.

___

This delegated mechanism is particularly useful for devices or low-resource environments where MetaMask is unavailable — or for advanced workflows involving backend coordination.


## Step 1: Authentication

To access and use the Dashboard, you first need to authenticate using MetaMask and your platform credentials.


### Install MetaMask

If MetaMask is not detected, you’ll see a prompt to install it.
Download it from the official website and add it to your browser.

![noMetamask](images/auth/noMetamask.png)

### Create an Account

Once MetaMask is installed and set up:
-	Enter your email and password to create an account on the platform.
-	This step links your MetaMask wallet to your platform account.

Make sure your wallet is unlocked in MetaMask.

![signUp](images/auth/signUp.png)

### Login

If you already have an account:
-	Enter your email and password in the login form.
-	MetaMask will automatically request permission to connect to the site — this is expected and required for secure interaction.

After successful login, you’ll be redirected to the main dashboard interface.

![logIn](images/auth/logIn.png)


# Step 2: Dashboard Layout Overview

After logging in, you’ll arrive at the main dashboard screen. Here’s what you’ll see:

### Sidebar Menu (Left Side)

On the left side of the screen, you’ll find the main navigation menu.
It gives you access to all key sections of the system, such as:
  -	Fleets
	-	Services
	-	Devices
	-	Verifiable Credentials (VCs)


At the top, there are two key icons:
	
1.	**Notifications Icon** - Displays system notifications related to transaction events.

2.	**Account Icon** - Shows:

- The first few characters of your connected wallet address.

-	The display name stored in the platform database (or undefined if this address hasn’t been used yet).

Clicking the account icon opens a dropdown with a Logout button, allowing you to safely disconnect from the platform.

# Step 3: Managing Addresses and Activity

**Table 1 – Your Addresses**

This table displays all the addresses you operate with, including:

-	Your connected wallet address
-	Any external addresses you’ve manually added via the form below (e.g., IoT devices)

These addresses are pulled from your wallet and the platform database.


**Table 2 – Recent Transactions**

When you select an address in Table 1, this table shows the latest 9 transactions associated with it.

This gives you a quick view of recent activity such as:

-	Credential operations
-	Device interactions
-	Payment events

![dash_main](images/dashboard/main.png)

**Form 1 – Add or Edit External Addresses**

Use this form to add external addresses (e.g., your devices) that you want to track or manage.

You’ll be asked to enter:
-	The address
-	A custom label/name for easier identification

You can also use this form to edit the label of an already-added address by re-submitting it with a new name.

![dash_addNewAddress](images/dashboard/addNewAddress.png)

# Step 4: Managing DIDs (Decentralized Identifiers)

This section of the Dashboard is dedicated to viewing and creating DIDs on the platform — primarily for devices.

**Address Cards**

At the top of the page, you’ll see a list of address cards, based on the addresses from Table 1 (see previous section).
Each card shows:

-	The custom label (name) of the address
-	Whether the address already has a DID
-	The current ETH balance (token support will be added later)

These cards provide a quick overview of your registered entities.

![did_main](images/dids/main.png)


**DID Creation Form**

Below the cards is a form for manual DID creation. Although DIDs are usually created automatically by the device software, this form is included in the Dashboard for flexibility and testing.

To create a new DID, fill in the following fields:

-	Address – the address that will be associated with the DID
-	Verification Method(s) (optional) – one or more addresses that can act on behalf of the DID (e.g., managers or service providers)
-	Information (optional) – any additional data or metadata you want to attach to the DID for identification or proof purposes.

📌 Note:
DIDs are only required for devices, not users. But the dashboard allows manual DID management in case automation fails or needs debugging.

![did_addNew](images/dids/addNew.png)


**Editing or Deleting a DID**

When you click on an address card, a dialog window will appear, allowing you to manage the associated DID.

From this dialog, you can:
-	Edit the verification method(s) – add or remove addresses authorized to act on behalf of the DID
-	Update the DID information – change or enrich metadata tied to the DID
-	Delete the DID – remove it entirely from the system

How it works:
- If you re-enter the same verification methods or info that are already associated — they will be removed.
-	If you enter new data — it will be added to the DID document.

 In production, these operations are expected to be handled automatically by the device firmware. The dashboard provides this manual control only as a fallback or for debugging/testing purposes.

![did_change](images/dids/change.png)


# Step 5: Managing Fleets


The Fleets tab allows you to create and manage groups of devices and services under a common structure. Each fleet has an owner, metadata, and a validity period.

![fleet_main](images/fleets/main.png)

## Create New Fleet

At the top of the page, you’ll see a “Create Fleet” button that opens a form.

To create a new fleet, fill out the following fields:

-	Owner Address – the address that will be the fleet’s administrator
-	Fleet Name – a label to help you identify the fleet
-	Data – optional information explaining the fleet’s purpose or use case (also used to help generate a unique fleet identifier)
-	Start & End Dates – the validity period of the fleet (e.g., when the fleet becomes active and when it expires)

You can choose to send the transaction directly or delegated

![fleet_newFleet](images/fleets/newFleet.png)

## Fleet Cards

Below the creation form, you’ll see cards for each fleet that your account manages. Each card displays:

-	The Fleet Name
-	Its Unique Fleet ID
-	The Owner’s Address
-	The Status (e.g., active, expired, or pending)

These cards offer a quick overview of your organizational structure

## Viewing Fleet Details


Clicking on any fleet card opens a detailed view with additional options and insights.

**Fleet Info Panel**

The panel shows all essential information about the selected fleet, along with several action buttons:

-	View Services – navigates to a page listing all services created within this fleet

-	Delete Fleet – opens a confirmation dialog to permanently remove the fleet from the system


**Transaction Table**

Below the action buttons, you’ll see a table that lists all relevant transactions related to this fleet, including:
-	Verifiable Credential (VC) creation and updates
-	Service additions or removals
-	Any actions tied to devices or addresses linked to the fleet

This gives you a transparent view of all blockchain-level activity within the fleet.

![fleet_card](images/fleets/card.png)


# Step 6: Managing Services

The Services tab lets you create and manage service units within your fleets. Each service belongs to a fleet and is managed by a designated address.

![service_main](images/services/main.png)


## Create New Service

At the top of the page, click the “Create Service” button to open the creation form.

To register a new service, fill out the following fields:

-	Fleet ID – the unique identifier of the fleet this service will belong to

-	Manager Address – the address that will control and operate the service

-	Data – optional metadata describing or verifying the purpose of the service

-	Info – the service name or label that will appear in the UI

-	Start & End Dates – the time range during which the service will be considered active

As with fleets, you can choose to submit this transaction directly or via delegated signature.

![service_new](images/services/newService.png)


## Service Cards

Below the form, the Dashboard displays cards for all services your account manages. Each card shows:

- Service Name (info field)
-	Service ID (a unique identifier)
-	Manager Address
-	Status (active, expired, etc.)

## Viewing Service Details

Clicking on a service card opens a detailed info panel:

-	It shows all metadata and status details
-	Includes the buttons to view VCs (Verifiable Credentials) created under this service
-	Provides a Delete Service button, opening a confirmation dialog

![service_card](images/services/card.png)


# Step 7: Managing Verifiable Credentials (VCs)

The VC tab provides a complete overview of all Verifiable Credentials registered on the platform, along with visual insights into their interactions.

![vc_main](images/vcs/main.png)

## VC Cards

At the top of the page, you’ll see cards for all VCs currently on the platform.

Each VC card displays:

-	The name of the VC
-	Its description
-	The associated service
-	The VC’s unique identifier
-	The DID address it is linked to

⚠️ Note: When a device creates a new VC, it will initially appear in the pending state, awaiting confirmation from the service manager. (This functionality will be available starting March 26th.)

## VC Graph Visualization

Below the cards is an interactive graph of your VCs.

-	Each node represents a VC.
-	An edge (connection) between two VCs means that they are interacting or involved in payments with each other.

This view helps you understand relationships and dependencies between devices/services at a glance.

## Viewing and Managing a VC

Clicking on a VC card opens a detailed panel where you can:
-	Edit the name or description of the VC
-	Delete the VC
- View a transaction/event table, listing all on-chain actions related to this VC’s identifier

This allows full lifecycle management of VCs directly through the Dashboard.

![vc_card](images/vcs/card.png)


![payment_main](images/payment/main.png)
![payment_exist_events](images/payment/exist_events.png)
![payment_exist_settings](images/payment/exist_settings.png)
![payment_exist_whiteList](images/payment/exist_whiteList.png)
![payment_create1](images/payment/create1.png)
![payment_create2](images/payment/create2.png)


![handle_main](images/handle/main.png)
![handle_card](images/handle/card.png)
![handle_signing](images/handle/signing.png)
![handle_validation](images/handle/validation.png)
![handle_payment](images/handle/payment.png)





