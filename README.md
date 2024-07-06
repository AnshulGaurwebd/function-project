# MetroTicketBooking
this is simple Metro ticket booking contract written in solidity
**Discription:**
The MetroTicketBooking Solidity contract allows users to purchase and manage metro tickets on the Ethereum blockchain. It has state variables to store the owner's address, the ticket price, the total number of tickets available, and a mapping to track the number of tickets owned by each user. The constructor initializes the contract with a specified ticket price and total number of tickets, setting the deployer as the owner. The contract provides several functions: buyTicket, which lets users buy tickets while ensuring the correct Ether amount is sent and that enough tickets are available; getTicketCount, which returns the number of tickets a user owns; cancelTicket, which allows users to cancel tickets and get a refund; and checkBalance, which returns the contract's Ether balance.

**Getting Started**

Open Remix IDE:
Visit Remix IDE in your web browser.

Create a New File:

In the Remix IDE, go to the "File Explorer" on the left sidebar.
Click on the "+" icon to create a new file.
Name your file MetroTicketBooking.sol.

the Solidity Code:

    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;

    contract MetroTicketBooking {
    address public owner;
    uint256 public ticketPrice;
    uint256 public totalTickets;
    mapping(address => uint256) public tickets;

    constructor(uint256 _ticketPrice, uint256 _totalTickets) {
        owner = msg.sender;
        ticketPrice = _ticketPrice;
        totalTickets = _totalTickets;
    }

    // Function to buy a ticket
    function buyTicket(uint256 count) public payable {
        if (count <= 0) {
            revert("You must buy at least one ticket.");
        }
        if (msg.value != ticketPrice * count) {
            revert("Incorrect amount of Ether sent.");
        }
        if (totalTickets < count) {
            revert("Not enough tickets available.");
        }

        tickets[msg.sender] += count;
        totalTickets -= count;
    }

    // Function to get the number of tickets owned by a user
    function getTicketCount() public view returns (uint256) {
        return tickets[msg.sender];
    }

    // Function to cancel tickets
    function cancelTicket(uint256 count) public {
        require(count > 0, "You must cancel at least one ticket.");
        require(tickets[msg.sender] >= count, "You do not have enough tickets to cancel.");

        tickets[msg.sender] -= count;
        totalTickets += count;
        payable(msg.sender).transfer(ticketPrice * count);
    }

    // Function to check the balance of the contract
    function checkBalance() public view returns (uint256) {
        assert(address(this).balance >= 0);
        return address(this).balance;
    }
    }

Compile the Contract:

Click on the "Solidity Compiler" tab on the left sidebar (it looks like a "G" shape).
Ensure the compiler version is set to a compatible version with your Solidity code (in this case, 0.8.0).
Click on the "Compile MetroTicketBooking.sol" button.

Deploy the Contract:

Click on the "Deploy & Run Transactions" tab on the left sidebar (it looks like an Ethereum diamond).
Ensure the "Environment" is set to "JavaScript VM" (this will use an in-browser blockchain for testing).
In the "Deploy" section, you will see the MetroTicketBooking contract with input fields for the constructor parameters (_ticketPrice and _totalTickets).
Enter values for _ticketPrice (e.g., 1000000000000000000 for 1 Ether) and _totalTickets (e.g., 100).
Click on the "Deploy" button.

Interact with the Contract:

Once deployed, the contract instance will appear in the "Deployed Contracts" section.
Expand the contract instance to see the available functions.
Buying Tickets:

To buy tickets, enter the number of tickets in the count field of the buyTicket function.
Enter the required Ether value in the "Value" field at the top (e.g., 2000000000000000000 for 2 tickets if 1 ticket costs 1 Ether).
Click on the buyTicket button to execute the transaction.

Checking Ticket Count:

Click on the getTicketCount function to see the number of tickets owned by the connected account.

Canceling Tickets:

Enter the number of tickets to cancel in the count field of the cancelTicket function.
Click on the cancelTicket button to execute the transaction.
Checking Contract Balance:

Click on the checkBalance function to see the current balance of the contract.
