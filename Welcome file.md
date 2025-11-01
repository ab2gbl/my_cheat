
# Mobile Payement Client Documentation
## Overview
The Mobile Payment Client Administration system serves as the back-office platform for both system users and banking institutions. It enables the management of clients who use the Mobile Payment service, whether through the mobile application or the web version.
This documentation provides a detailed description of each module within the system and outlines their functionalities.
## Acknowledgements
To effectively work on this project, you should have solid experience with HTML, CSS, Angular, and TypeScript, as well as familiarity with asynchronous tools such as RxJS and Signals .
If you are not yet proficient in any of these technologies, it is recommended that you review the following tutorials and learning resources before proceeding.
- [HTML](https://developer.mozilla.org/fr/docs/Web/HTML/Reference/Elements)
- [CSS](https://developer.mozilla.org/fr/docs/Web/CSS)
- [TYPESCRIPT](https://www.typescripttutorial.net/)
- [RXJS](https://www.learnrxjs.io/)
- [SIGNLAS](https://blog.angular-university.io/angular-signals/)
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
This document provides an overview of the folder structure for the Mobile Payment Project Client and a brief explanation of the purpose and functionality of each directory.
## @fuse
Contains all features and configurations related to the Fuse template used as the base for this project.
## app/
The main application folder containing the core logic, modules, and components.
### app/core
Dedicated to the authentication process.
- Guards – Protect routes and manage access control
- Interceptors – Handle and modify HTTP requests/responses.
- Services – Manage authentication-related logic.
### app/modules
This folder contains the main application modules, including:
- auth/
- admin/
 Each subfolder inside represents a distinct feature view within the application.
#### Admin Module Overview
The admin module is the main that composed of multiple feature components, each responsible for managing a specific part of the system.
#### admin/accounts/
Manages bank accounts.
Features:
- create/read/update account 
- View account details
- Make account as default
#### admin/payment/
employee generate QR codes to do payments
Features:
- Generate Qr code ( dynamic, static with/without amount )
- transactions history 
#### admin/transfer/
Client transferring money to other user 
Features:
- Select a beneficiary
- Transfer money to this beneficiary
- transactions history 
#### admin/transactions/
Displays the list of all transactions made 
- View transaction details and trace
- Filter transactions by criteria
- Employee can Refund transactions 
#### admin/stores/
Lists the stores.
Features:
- Full CRUD operations
- Filtering support
#### admin/employees/
Lists the employees.
Features:
- Full CRUD operations
- Filtering support
#### admin/roles/
Manages the roles of employees.
Features:
- Full CRUD operations
- Filtering support
- Role has name , description and privileges groups
	- Transactions
	- Stores
	- Employees
	- Accounts
	- Roles
- each group has mult privileges 
- privilege has 4 params:
	- Porte: same store the employee works on
	- Descendant: stores under his store
	- Employees: selecting specific employees
	- Stores: selecting specific stores

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ1NTI2NzkyNSwtMzMyNDU1MzYzXX0=
-->