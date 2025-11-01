
# Mobile Payement Client Documentation
## Overview
The Mobile Payment Client Administration system serves as the back-office platform for both system users and banking institutions. It enables the management of clients who use the Mobile Payment service, whether through the mobile application or the web version.
This documentation provides a detailed description of each module within the system and outlines their functionalities.
## Acknowledgements
To effectively work on this project, you should have solid experience with HTML, CSS, Angular, and TypeScript, as well as familiarity with asynchronous tools such as RxJS, Signals, and NgRx.
If you are not yet proficient in any of these technologies, it is recommended that you review the following tutorials and learning resources before proceeding.
- [HTML](https://developer.mozilla.org/fr/docs/Web/HTML/Reference/Elements)
- [CSS](https://developer.mozilla.org/fr/docs/Web/CSS)
- [TYPESCRIPT](https://www.typescripttutorial.net/)
- [RXJS](https://www.learnrxjs.io/)
- [SIGNLAS](https://blog.angular-university.io/angular-signals/)
- [NGRX](https://ngrx.io/guide/store)
## Environment Variables
To run this project, you should have these environment variables to your .env file
`apiAuthUrl` : for authentication requests
`apiUrl` : for getting ressources
## Installation
Install mobile payment with npm
```bash
 cd mobile-payment-web
 npm install
```
to display the documentation
```bash
 npm run compodoc
 compodoc -so
```
## Documentation
This document provides an overview of the folder structure for the Mobile Payment Project and a brief explanation of the purpose and functionality of each directory.
- ## @fuse
Contains all features and configurations related to the Fuse template used as the base for this project.
- ## app/
The main application folder containing the core logic, modules, and components.
- ### app/core
Dedicated to the authentication process.
- Guards – Protect routes and manage access control
- Interceptors – Handle and modify HTTP requests/responses.
- Services – Manage authentication-related logic.
- ### app/modules
This folder contains the main application modules, including:
- auth/
- admin/
- statistics/ 
 Each subfolder inside represents a distinct feature view within the application.
- #### Admin Module Overview
The admin module is the main that composed of multiple feature components, each responsible for managing a specific part of the system.
- #### admin/accounts/
Manages bank accounts.
Features:
	- create/read/update account 
	- View account details
	- Make account as default

- #### admin/paiment/
employee generate QR codes to do paiments
Features:
		- Generate Qr code ( dynamic, static with/without amount )
		- transactions history 
- #### admin/transactions/
Displays the list of all transactions made 
	- View transaction details and trace
	- Filter transactions by criteria
	- Employee can Refund transactions 
- #### admin/stores/
Lists the stores.
Features:
- Full CRUD operations
- Filtering support
- #### admin/stores/
Lists the stores.
Features:
- Full CRUD operations
- Filtering support


- #### admin/merchant-account/
Manages merchant bank accounts.
Features:
- View account details
- Suspend accounts
- Filter merchant accounts

- #### admin/region/
Displays regions associated with banks.
Notes:
- Regions only exist if the bank supports them (region = true).
 Features
- Full CRUD operations
- Filtering support
- #### admin/region-user/
Manages user regions.
Features:
- Full CRUD operations
- Filtering support
- #### admin/agencies/
Lists bank agencies.
Notes:
- Exists only for non-centralized banks.
 Features:
- Full CRUD operations
- Filtering support
- #### admin/agencies-user/
Displays user-linked agencies.
Features:
- Full CRUD operations
- Filtering support
- #### admin/stores/
Lists all stores linked to merchants.
Features:
- Full CRUD operations
- Filtering support
- #### admin/employees/
Displays store employees.
Features:
- Full CRUD operations
- Filtering support
- #### admin/users/
Lists all system users.
Features:
- Full CRUD operations
- Activate/Deactivate users via radio button
- Filtering support
- #### admin/roles/
Displays all system roles.
Features:
- Full CRUD operations
- Filtering support
- #### admin/sessions/
Displays login session history.
Features:
- Delete session entries
- Filter sessions
- #### admin/frauds/
Lists all blocked users.
Features:
- Unblock users
- #### admin/account-policies/
Displays bank account policies for clients.
Features:
- Full CRUD operations
- Filtering support
- #### admin/mcc-thresholds/
Lists merchant thresholds defined by the bank.
Features:
- Full CRUD operations
- Filtering support
- #### admin/notifications/
Manages notifications sent to users, clients, and merchants.
Features:
- Full CRUD operations
- Filtering support
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTM5NTQ3MTIyLC0zMzI0NTUzNjNdfQ==
-->