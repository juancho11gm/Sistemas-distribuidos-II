# Distributed systems - II
## Product sales project: 
The general objective of the final project of the ISD course is focused on the practical appropriation of the concepts related to the development of applications and the use of middlewares in distributed computing environments. This purpose is achieved by creating a distributed system that provides a solution to a real-world problem. The specific objectives are:

• Put into practice the concepts of distributed architectures, emphasizing some of the key problems of concurrency management: file management, concurrency control, transactional management and recovery from failures.

• Implement, through the use of RMI, a non-centralized system that achieves cooperation between processes by defining well-defined interaction protocols.

For this academic period, the project will focus on the management of social debit cards to manage aid to families in vulnerable situations. All the transaction management mechanisms of the system must operate in a distributed manner and guaranteeing compliance with ACID properties. All communications between processes must be done through the Java RMI middleware.

Description of the System to be Developed The project consists of developing and implementing a distributed application to support the national government in managing a consumer debit card for the vulnerable population, where a certain amount of money is posted monthly. Card beneficiaries can access a virtual store where they can make purchases at special prices. A client user, to interact with the machine that has the logic of the virtual store, always contacts via RMI two servers that consistently handle the objects involved in the transactions. The product catalog server handles all the information of the products that are in the virtual store, as well as transactional information when one or more users enter the platform. Inconsistencies can occur, when several customers concurrently order the same product. There may also be inconsistencies if new products are entered into the inventory concurrently with a purchase.

At the beginning the client opens his session identifying himself with the card number, then products and quantities are selected; finally, by confirming the order, a transaction is started. In the transaction, first, it is verified that there is a sufficient balance for the total payment of the
order.
If so, the inventory is assigned one by one of the products of the order; Before making the modification of each item, it must be verified that there is a sufficient quantity of requested products. If there is not enough stock of an item, it is removed from the order and the user is informed; the other items are still being processed. Finally, the money is deducted from the balance associated with the card; If items have been removed, the respective adjustment must be made.

Additionally, in the system there is an administrator user who can deposit money into the cards. Similarly, the administrator can enter the inventory of new products. These actions must have transactional management. It could be the case that just at the moment a purchase is being made, money enters the account or new products.

For greater ease, both the product catalog and the list of enabled cards are fixed. At server startup, this data is loaded from a text file; the balance is initialized for each card and the initial inventory is set for each product. The server for user management, which includes the management of cards and balances, operates on a separate machine; and the server for managing the product catalog and inventory runs on a separate machine. Users, both clients and administrators, connect from different machines.

In order to make the system more robust, access control management can be included as an “optional” component. The first time a client enters the system, they must generate a new password, the corresponding hash is generated and stored permanently, in order to make subsequent validations. To ensure the confidentiality of passwords encryption must be used.
Distributed Communication
Taking into account that in a transaction multiple products are usually worked and that there can be multiple simultaneous transactions, also using servers on different machines, it is likely that transactional problems will occur. Therefore, the 2PC two-phase atomic consummation protocol should be used to ensure consistency while complying with ACID properties. Similarly, there may be concurrency problems; therefore, an optimistic concurrency control protocol with forward validation should be used.

For transactional management, the management of 2PC must be integrated in coordination with that of concurrency control. All tentative copies of objects that are being used by different users should be managed. The status of the inventory of products and card balances must be permanently saved when committing transactions.

The system must include failure recovery mechanisms. When one of the servers stops working and later comes into operation, the reconnection must be done automatically, that is, it will be transparent to users; transactions should continue where they were left off and not start from scratch. During the development of transactions, information must be recorded on disk that allows the recovery of the product catalog server. It is left as an additional “optional” task to include fault recovery for the user server.

All interactions between processes associated with servers and users must be done through the use of RMI.
