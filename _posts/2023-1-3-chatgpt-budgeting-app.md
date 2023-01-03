---
layout: post
title: 'I asked Chat GPT to build a Budgeting App'
categories: blog
tags: ['Chat GPT', 'Budgeting App']
excerpt: 'SonarQube is a leading Code Quality Assurance tool which collects and analyzes your code and it provides the report, the post is all about getting started with it locally and using .Net Core'
date: December 29, 2022
author: self
---

# I asked Chat GPT to build a Budgeting App

Chat GPT is an AI-based chatbot that can answer any question, where it understands any topics and can do tasks like creating a Budgeting App (which I have asked ChatGPT to create) as well as writing blog posts, writing unit tests and what not.

So first thing I have asked is to explain, what is Chat GPT as a 5 year old

> GPT is a computer program that can write stories, answers, or even
> have a chat with you! It's really smart and it knows a lot of words
> and how to use them to make sentences that make sense.
> 
> Sometimes, you can give GPT a little bit of information, like a list
> of words or a sentence, and it can turn that into a whole story or a
> whole bunch of answers to questions. It's like a magic machine that
> can write lots of things just by learning from other things that
> people have written before.
> 
> GPT is really good at pretending to be a person when it talks to you.
> It can even translate words from one language to another. It's really
> helpful for making chatbots or for helping people write things.

The response is pretty awesome and I can explain the same thing to my kid too.

Next I have asked Chat GPT to write a budget app and given the probpts as below:

> can you give me the code to create a budget app that allows me to edit
> the items and store them in a database using javascript

The response I got is 

    const budgetForm = document.querySelector('#budget-form');
    const budgetInput = document.querySelector('#budget-input');
    const budgetList = document.querySelector('#budget-list');
    
    let budget = [];
    
    function addBudget(e) {
      e.preventDefault();
    
      if (budgetInput.value === '') return;
    
      const newBudget = {
        id: Date.now(),
        budget: budgetInput.value
      };
    
      budget.push(newBudget);
    
      budgetInput.value = '';
    
      updateBudget();
    }
    
    function removeBudget(id) {
      budget = budget.filter(budget => budget.id !== id);
    
      updateBudget();
    }
    
    function updateBudget() {
      budgetList.innerHTML = '';
    
      budget.forEach(budget => {
        budgetList.innerHTML += `
          <li class="budget-item" data-id="${budget.id}">
            ${budget.budget}
            <button class="btn btn-danger btn-sm float-right delete-budget-btn">
              Remove
            </button>
          </li>
        `;
      });
    
      localStorage.setItem('budget', JSON.stringify(budget));
    }
    
    budgetForm.addEventListener('submit', addBudget);
    budgetList.addEventListener('click', e => {
      if (e.target.classList.contains('delete-budget-btn')) {
        const id = e.target.parentElement.getAttribute('data-id');
    
        removeBudget(id);
      }
    });
    
    updateBudget();

So I have got what I wanted, but I wanted to have HTML, CSS and JavaScript too, so I again asked:

> can you give me the code to create a budget app having 50/30/20
> budgeting method (credit and debit) that allows me to edit the items
> and store them in a database using javascript, html, css

The response was

    <html>
    <head>
      <style>
        /* Some basic styling for the app */
        * {
          font-family: sans-serif;
        }
    
        form {
          display: flex;
          flex-direction: column;
          width: 50%;
          margin: 0 auto;
        }
    
        label {
          margin-bottom: 5px;
        }
    
        input[type="number"] {
          width: 60px;
          margin-right: 10px;
        }
    
        .transactions {
          display: flex;
          flex-direction: column;
          width: 50%;
          margin: 20px auto;
        }
    
        .transactions h2 {
          text-align: center;
        }
    
        .transactions ul {
          list-style: none;
          padding: 0;
          margin: 0;
        }
    
        .transaction {
          display: flex;
          justify-content: space-between;
          align-items: center;
          border-bottom: 1px solid #ccc;
          padding: 5px 0;
        }
    
        .transaction.credit {
          color: green;
        }
    
        .transaction.debit {
          color: red;
        }
    
        .transaction-type {
          width: 75px;
          text-align: center;
        }
    
        .transaction-amount {
          width: 75px;
          text-align: right;
        }
    
        .transaction-category {
          width: 75px;
          text-align: center;
        }
    
        .transaction-delete {
          cursor: pointer;
          width: 20px;
          text-align: center;
        }
      </style>
    </head>
    <body>
      <!-- The form to add new transactions -->
      <form id="transaction-form">
        <label for="transaction-type">Type:</label>
        <select id="transaction-type">
          <option value="credit">Credit</option>
          <option value="debit">Debit</option>
        </select>
        <label for="transaction-amount">Amount:</label>
        <input type="number" id="transaction-amount" min="0" step="0.01">
        <label for="transaction-category">Category:</label>
        <select id="transaction-category">
          <option value="necessary">Necessary</option>
          <option value="essential">Essential</option>
          <option value="discretionary">Discretionary</option>
        </select>
        <button type="submit">Add</button>
      </form>
    
      <!-- The container to display the current balance and the transactions -->
      <div class="transactions">
        <h2>Current Balance: <span id="balance">0</span></h2>
        <ul id="transactions"></ul>
      </div>
    
      <script>
        const transactionForm = document.querySelector('#transaction-form');
        const transactionType = document.querySelector('#transaction-type');
        const transactionAmount = document.querySelector('#transaction-amount');

So I did not got the full code, JavaScript was missing and got only CSS and HTML. So I asked the ChatGPT to provide only the JavaScript code now,

> can you give javascript only of the above code

    const transactionForm = document.querySelector('#transaction-form');
    const transactionType = document.querySelector('#transaction-type');
    const transactionAmount = document.querySelector('#transaction-amount');
    const transactionCategory = document.querySelector('#transaction-category');
    const transactionsList = document.querySelector('#transactions');
    const balance = document.querySelector('#balance');
    
    // An array to store the transactions
    let transactions = [];
    
    // A helper function to calculate the balance
    function calculateBalance() {
      let total = 0;
    
      transactions.forEach(transaction => {
        if (transaction.type === 'credit') {
          total += transaction.amount;
        } else {
          total -= transaction.amount;
        }
      });
    
      balance.textContent = total.toFixed(2);
    }
    
    // A function to add a new transaction
    function addTransaction(e) {
      e.preventDefault();
    
      if (transactionAmount.value === '') return;
    
      const transaction = {
        id: Date.now(),
        type: transactionType.value,
        amount: +transactionAmount.value,
        category: transactionCategory.value
      };
    
      transactions.push(transaction);
    
      transactionAmount.value = '';
    
      updateTransactions();
    }
    
    // A function to remove a transaction
    function removeTransaction(id) {
      transactions = transactions.filter(transaction => transaction.id !== id);
    
      updateTransactions();
    }
    
    // A function to update the transactions list and the balance
    function updateTransactions() {
      transactionsList.innerHTML = '';
    
      transactions.forEach(transaction => {
        const className = transaction.type === 'credit' ? 'credit' : 'debit';
    
        transactionsList.innerHTML += `
          <li class="transaction ${className}" data-id="${transaction.id}">
            <div class="transaction-type">${transaction.type}</div>
            <div class="transaction-amount">${transaction.amount}</div>
            <div class="transaction-category">${transaction.category}</div>
            <div class="transaction-delete">
              <button class="btn btn-danger btn-sm delete-transaction-btn">
                X
              </button>
            </div>
          </li>
        `;
      });
    
      calculateBalance();
    
      localStorage.setItem('transactions', JSON.stringify(transactions));
    }
    
    // Event listeners
    transactionForm.addEventListener('submit', addTransaction);
    transactionsList.addEventListener('click', e => {
      if (e.target.classList.contains('delete-transaction-btn')) {
        const id = e.target.parentElement.parentElement.getAttribute('data-id');
    
        removeTransaction(id);
      }
    });
    
    updateTransactions();

After that I have done some modifications and created the full code as below:

    <html>
    <head>
      <style>
        /* Some basic styling for the app */
        * {
          font-family: sans-serif;
        }
    
        form {
          display: flex;
          flex-direction: column;
          width: 50%;
          margin: 0 auto;
        }
    
        label {
          margin-bottom: 5px;
        }
    
        input[type="number"] {
          width: 60px;
          margin-right: 10px;
        }
    
        .transactions {
          display: flex;
          flex-direction: column;
          width: 50%;
          margin: 20px auto;
        }
    
        .transactions h2 {
          text-align: center;
        }
    
        .transactions ul {
          list-style: none;
          padding: 0;
          margin: 0;
        }
    
        .transaction {
          display: flex;
          justify-content: space-between;
          align-items: center;
          border-bottom: 1px solid #ccc;
          padding: 5px 0;
        }
    
        .transaction.credit {
          color: green;
        }
    
        .transaction.debit {
          color: red;
        }
    
        .transaction-type {
          width: 75px;
          text-align: center;
        }
    
        .transaction-amount {
          width: 75px;
          text-align: right;
        }
    
        .transaction-category {
          width: 75px;
          text-align: center;
        }
    
        .transaction-delete {
          cursor: pointer;
          width: 20px;
          text-align: center;
        }
      </style>
    </head>
    <body>
      <!-- The form to add new transactions -->
      <form id="transaction-form">
        <label for="transaction-type">Type:</label>
        <select id="transaction-type">
          <option value="credit">Credit</option>
          <option value="debit">Debit</option>
        </select>
        <label for="transaction-amount">Amount:</label>
        <input type="number" id="transaction-amount" min="0" step="0.01">
        <label for="transaction-category">Category:</label>
        <select id="transaction-category">
          <option value="necessary">Necessary</option>
          <option value="essential">Essential</option>
          <option value="discretionary">Discretionary</option>
        </select>
        <button type="submit">Add</button>
      </form>
    
      <!-- The container to display the current balance and the transactions -->
      <div class="transactions">
        <h2>Current Balance: <span id="balance">0</span></h2>
        <ul id="transactions"></ul>
      </div>
    	<script>
    		const transactionForm = document.querySelector('#transaction-form');
    		const transactionType = document.querySelector('#transaction-type');
    		const transactionAmount = document.querySelector('#transaction-amount');
    		const transactionCategory = document.querySelector('#transaction-category');
    		const transactionsList = document.querySelector('#transactions');
    		const balance = document.querySelector('#balance');
    
    		// An array to store the transactions
    		let transactions = [];
    
    		// A helper function to calculate the balance
    		function calculateBalance() {
    		  let total = 0;
    
    		  transactions.forEach(transaction => {
    			if (transaction.type === 'credit') {
    			  total += transaction.amount;
    			} else {
    			  total -= transaction.amount;
    			}
    		  });
    
    		  balance.textContent = total.toFixed(2);
    		}
    
    		// A function to add a new transaction
    		function addTransaction(e) {
    		  e.preventDefault();
    
    		  if (transactionAmount.value === '') return;
    
    		  const transaction = {
    			id: Date.now(),
    			type: transactionType.value,
    			amount: +transactionAmount.value,
    			category: transactionCategory.value
    		  };
    
    		  transactions.push(transaction);
    
    		  transactionAmount.value = '';
    
    		  updateTransactions();
    		}
    
    		// A function to remove a transaction
    		function removeTransaction(id) {
    		  transactions = transactions.filter(transaction => transaction.id !== id);
    
    		  updateTransactions();
    		}
    
    		// A function to update the transactions list and the balance
    		function updateTransactions() {
    		  transactionsList.innerHTML = '';
    
    		  transactions.forEach(transaction => {
    			const className = transaction.type === 'credit' ? 'credit' : 'debit';
    
    			transactionsList.innerHTML += `
    			  <li class="transaction ${className}" data-id="${transaction.id}">
    				<div class="transaction-type">${transaction.type}</div>
    				<div class="transaction-amount">${transaction.amount}</div>
    				<div class="transaction-category">${transaction.category}</div>
    				<div class="transaction-delete">
    				  <button class="btn btn-danger btn-sm delete-transaction-btn">
    					X
    				  </button>
    				</div>
    			  </li>
    			`;
    		  });
    
    		  calculateBalance();
    
    		  localStorage.setItem('transactions', JSON.stringify(transactions));
    		}
    
    		// Event listeners
    		transactionForm.addEventListener('submit', addTransaction);
    		transactionsList.addEventListener('click', e => {
    		  if (e.target.classList.contains('delete-transaction-btn')) {
    			const id = e.target.parentElement.parentElement.getAttribute('data-id');
    
    			removeTransaction(id);
    		  }
    		});
    
    		updateTransactions();
    	</script>
    </body>
    </html>

And thus my Budgeting App is Ready

![Budgeting App](https://gopeshsharma.dev/images/blog/budgeting-app.png)

And at last I have asked Chat GPT to provide a summary of the above code

>can you write a summary of the above code

And the response was:

> The budget app has a form to add new transactions, which includes a
> dropdown to select the type of transaction (credit or debit), an input
> field for the amount, and a dropdown to select the category
> (necessary, essential, or discretionary).
> 
> The app also has a container to display the current balance and the
> list of transactions. The balance is calculated by adding up the
> credit transactions and subtracting the debit transactions.
> 
> The app stores the transactions in an array and uses event listeners
> to handle form submissions and clicks on the delete buttons. The
> `updateTransactions` function is used to update the list of
> transactions and the balance whenever a new transaction is added or an
> existing transaction is removed. The function also stores the updated
> transactions in the browser's local storage.

So kudos to the bot which can understand my query and provided me on what I was looking for.
