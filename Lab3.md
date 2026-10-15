# Lab 3

# Blaze Megyesi-Wadley





Note: I was told by my group afterwards that we only needed to do 5 Use Cases... wish I knew that before deciding 17 would be "fun" -_-

---

## 1. Actors
 
| **Customer** | 

A person who buys books from EBook over the Internet and pays by credit card. Can search the catalogue, read and write book comments, check an order's status, cancel an order that has not been sent out, and return a book within a week.


| **Clerk** | 

An EBook employee. Enters phone and mail orders for customers, searches the catalogue, reads and writes comments, checks order status, maintains the catalogue, books pending from a publisher, and sends received books on to customers.


| **Publisher** | 

An external company whose system EBook communicates with directly. Receives book orders and cancellation instructions from EBook, and ships ordered books to the EBook store.




---

## 2. Use Cases and Priorities

| ID | Use case | Primary actor | Priority |
|----|----------|---------------|----------|
| UC1 | Search Catalog | Customer, Clerk | High |
| UC2 | Place Order | Customer, Clerk | High |
| UC3 | Verify Payment (included in UC2) | Customer, Clerk (UC2) | High |
| UC4 | Order Book from Publisher (included in UC2) | Customer, Clerk (UC2). Publisher is secondary | High |
| UC5 | Ship Book to Store | Publisher | High |
| UC6 | Send Book to Customer | Clerk | High |
| UC14 | Add Book to Catalog | Clerk | High |
| UC7 | Check Order Status | Customer, Clerk | Medium |
| UC8 | Cancel Order | Customer | Medium |
| UC9 | Cancel Publisher Order (included in UC8) | Customer (UC8). Publisher is secondary | Medium |
| UC10 | Return Book | Customer | Medium |
| UC11 | Issue Refund (included in UC10) | Customer (UC10) | Medium |
| UC15 | Update Book Information | Clerk | Medium |
| UC16 | Delete Book from Catalog | Clerk | Medium |
| UC12 | Add Book Comment | Customer, Clerk | Low |
| UC13 | View Book Comments | Customer, Clerk | Low |
| UC17 | Check Pending Books from Publisher | Clerk | Low |



---

## 3. Use Case Descriptions

### Use Case UC1. Search Catalog
**Primary actor:** Customer, Clerk
**Description:** The actor searches the book catalogue by ISBN, title, author, publisher, or other catalogue information.
**Pre-condition:** The catalogue contains at least one book.
**Post-condition:** A list of matching books is displayed.


**Main scenario:**
1. The actor opens the catalogue search page.
2. The actor enters one or more search functions.
3. The system searches the catalogue.
4. The system displays the matching books with ISBN, title, authors, publisher, and price.
5. The actor selects a book to see its full details.


**Extensions:**
- 4a. No book matches: the system tells the actor that nothing was found and suggests changing the search input.
- 2a. The actor submits empty criteria: the system asks for at least one search input.




---

### Use Case UC2. Place Order
**Primary actor:** Customer, Clerk (customer who phones)
**Description:** The actor orders one or more books and pays by credit card. The payment is verified first, then each book is ordered from its publisher.
**Pre-condition:** The books being ordered exist in the catalogue. The customer must be identified.
**Post-condition:** An order is recorded, payment is accepted, and an order for each book has been sent to its publisher. The customer has an order number.



**Main scenario:**
1. The actor adds one or more books to the order.
2. The actor enters the credit card and shipping details.
3. The actor confirms the order.
4. The system verifies the payment (UC3).
5. The system orders each book from its publisher (UC4).
6. The system records the order and displays an order number.




**Extensions:**
- 4a. Payment is declined: the system rejects the order, does not contact any publisher, and asks the customer to use another card.
- 5a. A publisher's system is unavailable: the order stays recorded as pending and the system retries later.
- 1a. The customer tries to submit an empty order: the system asks for at least one book.





---

### Use Case UC3. Verify Payment
**Primary actor:** Customer or Clerk (through UC2)
**Description:** The system checks that the credit card payment for an order is accepted.
**Pre-condition:** An order with credit card details is being placed (UC2).
**Post-condition:** The payment is marked as accepted or declined.



**Main scenario:**
1. The system takes the card details and order total from UC2.
2. The system sends an authorization request to the credit card processor.
3. The processor approves the payment.
4. The system marks the payment as accepted and returns control to UC2.



**Extensions:**
- 3a. The card is declined or invalid: the system marks the payment as declined and returns this to UC2.
- 2a. The processor does not respond: the system reports a payment error. UC2 does not continue.




---

### Use Case UC4. Order Book from Publisher
**Primary actor:** Customer or Clerk (UC2). The Publisher would be the secondary actor
**Description:** For each book in an accepted order, the system sends an order directly to the book's publisher.
**Pre-condition:** The payment for the order has been accepted (UC3).
**Post-condition:** Each book has a publisher order that is acknowledged by the publisher, and the books are counted as pending.



**Main scenario:**
1. The system groups the books in the order by publisher.
2. The system sends an order to each publisher's system.
3. The publisher's system acknowledges the order.
4. The system marks each book as pending from that publisher.



**Extensions:**
- 3a. The publisher rejects a book: the system marks it as unavailable and notifies the customer.
- 2a. The publisher's system is unavailable: the system queues the order and retries.




---

### Use Case UC5. Ship Book to Store
**Primary actor:** Publisher
**Description:** The publisher ships an ordered book to the EBook store, and the system records its arrival.
**Pre-condition:** A publisher order exists for the book (UC4).
**Post-condition:** The book is recorded as received at the store and is ready to be sent to the customer.




**Main scenario:**
1. The publisher ships the ordered book to the EBook store.
2. The publisher's system notifies the system that the book has shipped.
3. The book arrives and the system records it as received.
4. The system updates the status of the matching order.




**Extensions:**
- 1a. The publisher ships only some of the ordered books: the system records only the books received and the others remain pending.
- 3a. A book is damaged or incorrect: the system flags the order line and a clerk contacts the publisher.



---

### Use Case UC6. Send Book to Customer
**Primary actor:** Clerk
**Description:** When a book has arrived at the store, the clerk sends it to the customer. Can result in several shipments.
**Pre-condition:** The book is recorded as received at the store (UC5) and its order has not been cancelled.
**Post-condition:** The book is marked as sent to the customer and the order status is updated.



**Main scenario:**
1. The clerk opens the list of received books waiting to be sent.
2. The clerk selects a received book and its order.
3. The system shows the customer's shipping address.
4. The clerk packages the book and sends it.
5. The clerk confirms the shipment.
6. The system marks the book as sent and updates the order status.



**Extensions:**
- 2a. The order was cancelled before the book arrived: the system tells the clerk not to send it and the book is handled through UC9.
- 6a. This was the last book of the order: the system marks the whole order as complete.




---

### Use Case UC7. Check Order Status
**Primary actor:** Customer, Clerk
**Description:** The actor looks up the current status of an order, including which books have been sent and which are still pending.
**Pre-condition:** The order exists. A customer can only view their own orders.
**Post-condition:** The current status is displayed.




**Main scenario:**
1. The actor enters an order number. It's selected from an order list.
2. The system finds the order.
3. The system displays the status of each book in the order (pending, received, sent, cancelled).



**Extensions:**
- 2a. The order number is not found: the system shows an error and asks the actor to try again.
- 2b. A customer tries to look up another customer's order: the system denies access.




---

### Use Case UC8. Cancel Order
**Primary actor:** Customer
**Description:** The customer cancels an order only if it has not yet been sent out.
**Pre-condition:** The order exists and none of its books has been sent to the customer yet (books send can be returned through UC10).
**Post-condition:** The order is cancelled, the publisher is told (UC9), and the customer's payment is released.



**Main scenario:**
1. The customer selects the order to cancel.
2. The system checks that the order has not been sent.
3. The customer confirms the cancellation.
4. The system cancels the order with the publisher (UC9).
5. The system cancels or refunds the payment and marks the order as cancelled.
6. The system confirms the cancellation to the customer.



**Extensions:**
- 2a. The order has already been sent: the system refuses the cancellation and points the customer to the return process (UC10).
- 2b. Only some books have been sent: the system cancels only the unsent books.




---

### Use Case UC9. Cancel Publisher Order
**Primary actor:** Customer (UC8). The Publisher is the secondary actor
**Description:** EBook either sends the publisher a cancel instruction, or returns the books to the publisher if they have already arrived at the store.
**Pre-condition:** A cancelled order has books ordered from a publisher.
**Post-condition:** The publisher no longer owes EBook a delivery for the cancelled books, or the books have been returned along with the cancelation notice.



**Main scenario:**
1. The system finds the publisher orders linked to the cancelled order.
2. For books not yet shipped, the system sends a cancel instruction to the publisher.
3. The publisher's system confirms the cancellation.
4. The system removes the books from the pending list.



**Extensions:**
- 2a. The book(s) has already arrived at the store: the system creates a return of itmes to the publisher along with the cancel instructions.
- 3a. The publisher cannot cancel because the book has already shipped: the system treats the book as received when it arrives and returns it to the publisher.




---

### Use Case UC10. Return Book
**Primary actor:** Customer
**Description:** The customer returns a book within a week of receiving it and gets their money back.
**Pre-condition:** The customer received the book less than a week ago.
**Post-condition:** The return is recorded and the customer is refunded (UC11).



**Main scenario:**
1. The customer selects the order and the book to return.
2. The system checks that the return is within one week of delivery.
3. The customer chooses how to be refunded (card or check).
4. The system records the return and gives the customer return instructions.
5. The system issues the refund (UC11).
6. The system confirms the return to the customer.



**Extensions:**
- 2a. More than a week has passed: the system refuses the return.
- 5a. The returned book does not arrive at EBook: the refund is held until the book is received.




---

### Use Case UC11. Issue Refund
**Primary actor:** Customer (UC10)
**Description:** The system returns the customer's money for a returned book, gives back money via. credit card or check.
**Pre-condition:** A valid return has been recorded (UC10), with the customer's chosen refund method.
**Post-condition:** The refund is issued and recorded against the order.



**Main scenario:**
1. The system calculates the refund amount for the returned book.
2. The system reads the refund method chosen in UC10.
3. The system credits the amount to the customer's credit card.
4. The system records the refund and returns control to UC10.



**Extensions:**
- 2a. The customer chose a refund check: the system creates a refund check request for the customer's address instead of crediting the card.
- 3a. The card is closed or the credit fails: the system switches to a refund check.




---

### Use Case UC12. Add Book Comment
**Primary actor:** Customer, Clerk
**Description:** The actor enters a comment on a book in the catalogue.
**Pre-condition:** The book exists in the catalogue and the actor is identified.
**Post-condition:** The comment is saved and attached to the book.



**Main scenario:**
1. The actor opens a book's details page.
2. The actor chooses to add a comment.
3. The actor enters the comment text.
4. The actor submits the comment.
5. The system saves the comment with the actor's name and the date.



**Extensions:**
- 4a. The comment is empty: the system asks the actor to enter text.
- 4b. The comment is longer than the allowed length: the system asks the actor to shorten it.




---

### Use Case UC13. View Book Comments
**Primary actor:** Customer, Clerk
**Description:** The actor reads the comments that have been entered for a book.
**Pre-condition:** The book exists in the catalogue.
**Post-condition:** The comments are displayed. No data is changed.



**Main scenario:**
1. The actor opens a book's details page.
2. The actor chooses to view comments.
3. The system displays the comments for the book, newest first, with author and date.



**Extensions:**
- 3a. The book has no comments: the system says no comments have been entered yet.




---

### Use Case UC14. Add Book to Catalog
**Primary actor:** Clerk
**Description:** The clerk adds a new book to the catalogue.
**Pre-condition:** The clerk is logged in. The book is not already in the catalogue.
**Post-condition:** The new book can be found in the catalogue and ordered.



**Main scenario:**
1. The clerk opens the catalogue maintenance page and chooses to add a book.
2. The clerk enters the ISBN, title, authors, publisher, price, and other information.
3. The clerk submits the new book.
4. The system validates the information.
5. The system saves the book and confirms.



**Extensions:**
- 4a. The ISBN already exists: the system rejects the book and offers to open the existing entry.
- 4b. Required information is missing or invalid: the system highlights the problems and the clerk corrects them.




---

### Use Case UC15. Update Book Information
**Primary actor:** Clerk
**Description:** The clerk changes the information of an existing book, such as its price or publisher.
**Pre-condition:** The clerk is logged in and the book exists in the catalogue.
**Post-condition:** The book's information is updated.



**Main scenario:**
1. The clerk searches for the book (UC1) and opens it for editing.
2. The clerk changes one or more fields.
3. The clerk submits the changes.
4. The system validates the new values.
5. The system saves the changes and confirms.



**Extensions:**
- 4a. A new value is invalid: the system rejects the change and shows the reason.
- 3a. The clerk cancels: the system keeps the old information.
- 5a. The price changed: existing orders keep the price they were placed at.




---

### Use Case UC16. Delete Book from Catalog
**Primary actor:** Clerk
**Description:** The clerk removes a book from the catalogue.
**Pre-condition:** The clerk is logged in and the book exists in the catalogue.
**Post-condition:** The book no longer appears in catalogue searches.



**Main scenario:**
1. The clerk searches for the book (UC1) and selects it.
2. The clerk chooses to delete the book.
3. The system asks for confirmation.
4. The clerk confirms.
5. The system removes the book from the catalogue and confirms.



**Extensions:**
- 4a. The clerk does not confirm: nothing is deleted.
- 5a. The book is part of an order that is still open: the system warns the clerk and keeps the book's record for those orders, hiding it only from searches.




---

### Use Case UC17. Check Pending Books from Publisher
**Primary actor:** Clerk
**Description:** The clerk finds out how many books are still pending from a specific publisher.
**Pre-condition:** The clerk is logged in.
**Post-condition:** The number of pending books is displayed. No data is changed.



**Main scenario:**
1. The clerk opens the publisher inquiry page.
2. The clerk selects a publisher.
3. The system counts the books ordered from that publisher that have not yet been received.
4. The system displays the total and the list of pending books with their order numbers.



**Extensions:**
- 3a. Nothing is pending: the system displays zero.
- 2a. The publisher is not found: the system asks the clerk to select a publisher from the list.




---

## 4. Development Risks (prioritized)



| Priority | Risk | Why it matters / mitigation |
|----------|------|-----------------------------|


| 1 (High) | **Integration with publishers' systems.** 

Every publisher has its own system and interface, EBook depends on them for ordering, cancelling and shipping notices. | The business does not work if these links are unreliable or poorly specified. Can be mitigated by agreeing on the interface early, building one publisher first, and queuing and retrying failed requests. |


| 2 (High) | **Payment security and credit card handling.** 

The system handles card numbers and refunds. | A mistake will be costly in money and reputation. Mitigated by using a certified payment processor so card numbers are not stored, and by following official requirements. |


| 3 (Medium) | **Complex order states.** 

One order can have several books in different states (pending, received, sent, cancelled, returned) and ship in several parts. | This is easy to get wrong and causes incorrect status, cancellations, and refunds. Mitigated by defining the state model early and testing combinations of states. |


| 4 (Low) | **Catalogue data quality.** 

The catalogue is large and maintained by hand by clerks. | Wrong prices or ISBNs lead to wrong orders. Mitigated by validating input and checking ISBNs. |




---

## 5. High-Level Requirements Not Captured in the Use Case Model

1. **Performance:** Catalogue searches should return results within 2 seconds, and the system should support at least 500 customers browsing at the same time.
2. **Security:** Credit card information must be sent over an encrypted connection and must not be stored by EBook. Customers must only be able to see and change their own orders, and only clerks can maintain the catalogue.
3. **Availability:** The online store should be available 99.5% of the time since customers can order at any hour.
4. **Timing of returns and cancellations:** Returns are only accepted within 7 days of delivery and cancellations only before an order is sent out.
5. **Usability:** A customer should be able to place an order without help or training and the website should work on the common web browsers.
