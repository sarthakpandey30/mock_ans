# Mock Interview Answers for SWE Intern (Python)

## **Answer 1: Movie Ticket Booking System**

### Classes Defined:
- **Theater**: Manages the overall theater layout and booking logic.
- **Seat**: Represents an individual seat and its booking status.

### Assumptions Made:
- The theater is organized in rows and columns of seats.
- A seat is either "Available" or "Booked".

### Test Cases:
- Book a seat and verify its status.
- Cancel a booking and verify the seat status is reset.
- Attempt to book an already booked seat.

```python
class Seat:
    def __init__(self):
        self.status = "Available"

    def book(self):
        if self.status == "Available":
            self.status = "Booked"
            return True
        return False

    def cancel_booking(self):
        if self.status == "Booked":
            self.status = "Available"

class Theater:
    def __init__(self, rows, seats_per_row):
        self.seats = [[Seat() for _ in range(seats_per_row)] for _ in range(rows)]

    def display_seats(self):
        for i, row in enumerate(self.seats):
            print(f"Row {i + 1}: {' '.join(seat.status for seat in row)}")

    def book_seat(self, row, seat):
        if self.seats[row][seat].book():
            print(f"Seat {row + 1}-{seat + 1} booked successfully.")
        else:
            print(f"Seat {row + 1}-{seat + 1} is already booked.")

    def cancel_seat_booking(self, row, seat):
        self.seats[row][seat].cancel_booking()
        print(f"Booking for seat {row + 1}-{seat + 1} cancelled.")

# Test cases
theater = Theater(3, 3)
# Book seat (1, 1) and check
theater.book_seat(0, 0)
theater.display_seats()  # Seat should be booked

# Try to book the same seat
theater.book_seat(0, 0)  # Should indicate already booked

# Cancel booking and recheck
theater.cancel_seat_booking(0, 0)
theater.display_seats()  # Seat should be available again
```

---

## **Answer 2: Simple Banking System**

### Classes Defined:
- **BankAccount**: Represents individual bank accounts.
- **Bank**: Manages multiple accounts and provides services like transferring money between accounts.

### Assumptions Made:
- A unique account number is assigned automatically.
- Transaction history is maintained for each account.

### Test Cases:
- Create a bank account and verify initial balance.
- Perform deposits and withdrawals and verify the updated balance.
- Verify transaction history after operations.

```python
class BankAccount:
    account_counter = 1

    def __init__(self, holder_name, initial_balance=0):
        self.account_number = BankAccount.account_counter
        BankAccount.account_counter += 1
        self.holder_name = holder_name
        self.balance = initial_balance
        self.transaction_history = []

    def deposit(self, amount):
        self.balance += amount
        self.transaction_history.append(f"Deposited {amount}")

    def withdraw(self, amount):
        if self.balance >= amount:
            self.balance -= amount
            self.transaction_history.append(f"Withdrew {amount}")
        else:
            print("Insufficient funds.")

    def show_transaction_history(self):
        print(f"Transaction history for {self.holder_name}:")
        for transaction in self.transaction_history:
            print(transaction)

class Bank:
    def __init__(self):
        self.accounts = []

    def create_account(self, holder_name, initial_balance=0):
        account = BankAccount(holder_name, initial_balance)
        self.accounts.append(account)
        print(f"Account created for {holder_name} with Account Number: {account.account_number}")
        return account

# Test cases
bank = Bank()
account = bank.create_account("Alice", 1000)
account.deposit(500)
account.withdraw(200)
account.show_transaction_history()  # Should list deposit and withdrawal
```

---

## **Answer 3: Warehouse Capacity Management System**

### Classes Defined:
- **Warehouse**: Represents an individual warehouse with its capacity.
- **OrderFulfillment**: Handles the logic to find warehouse combinations.

### Assumptions Made:
- Warehouses have distinct capacities.
- The order should be fully fulfilled if possible, else an error is returned.

### Test Cases:
- Add multiple warehouses and ensure they are correctly stored.
- Given an order quantity, find the correct combination of warehouses.

```python
class Warehouse:
    def __init__(self, warehouse_id, capacity):
        self.warehouse_id = warehouse_id
        self.capacity = capacity

class OrderFulfillment:
    def __init__(self):
        self.warehouses = []

    def add_warehouse(self, warehouse_id, capacity):
        self.warehouses.append(Warehouse(warehouse_id, capacity))

    def find_warehouses_for_order(self, order_quantity):
        self.warehouses.sort(key=lambda w: w.capacity, reverse=True)
        combinations = []

        for warehouse in self.warehouses:
            if order_quantity <= 0:
                break
            if warehouse.capacity <= order_quantity:
                combinations.append(warehouse.warehouse_id)
                order_quantity -= warehouse.capacity
            else:
                combinations.append(warehouse.warehouse_id)
                break

        return combinations if order_quantity == 0 else "Order cannot be fully fulfilled."

# Test cases
order_system = OrderFulfillment()
order_system.add_warehouse("Warehouse1", 1000)
order_system.add_warehouse("Warehouse2", 700)
order_system.add_warehouse("Warehouse3", 500)
result = order_system.find_warehouses_for_order(1700)
print("Selected warehouses:", result)  # Expected: ['Warehouse1', 'Warehouse2']
```

---

## **Answer 4: Social Media User System**

### Classes Defined:
- **User**: Represents individual users.
- **Post**: Stores the content and metadata of user posts.

### Assumptions Made:
- Posts are stored in chronological order.
- Users can follow or unfollow other users.

### Test Cases:
- Create users and post messages.
- Verify feeds display both self and followed users' posts.
- Test follow and unfollow functionalities.

```python
class Post:
    def __init__(self, content):
        self.content = content

class User:
    def __init__(self, username):
        self.username = username
        self.posts = []
        self.following = set()

    def post_message(self, content):
        self.posts.insert(0, Post(content))

    def follow(self, user):
        self.following.add(user)

    def unfollow(self, user):
        self.following.discard(user)

    def view_feed(self):
        feed = [post.content for post in self.posts]
        for user in self.following:
            feed.extend(post.content for post in user.posts)
        return feed

# Test cases
alice = User("Alice")
bob = User("Bob")

alice.post_message("Alice's first post")
bob.post_message("Bob's first post")
alice.follow(bob)

feed = alice.view_feed()
print("Alice's feed:", feed)  # Should contain posts from Alice and Bob
```

---

## **Answer 5: Team Selection System**

### Classes Defined:
- **Player**: Represents individual players.
- **TeamBuilder**: Manages the player pool and generates team combinations.

### Assumptions Made:
- Each player has a name and a position.
- Combinations of players are generated using Python's itertools.

### Test Cases:
- Add players to the pool.
- Generate team combinations and validate the output size.

```python
import itertools

class Player:
    def __init__(self, name, position):
        self.name = name
        self.position = position

class TeamBuilder:
    def __init__(self):
        self.players = []

    def add_player(self, name, position):
        self.players.append(Player(name, position))

    def generate_team_combinations(self, team_size):
        return list(itertools.combinations(self.players, team_size))

# Test cases
team_builder = TeamBuilder()
team_builder.add_player("Alice", "Forward")
team_builder.add_player("Bob", "Midfielder")
team_builder.add_player("Charlie", "Defender")

combinations = team_builder.generate_team_combinations(2)
print("Team combinations:", combinations)  # Expect pairs of players
```

---

## **Answer 6: Polynomial Expression Calculator**

### Classes Defined:
- **Polynomial**: Represents a polynomial expression.
- **PolynomialOperation**: Encapsulates arithmetic operations on polynomials.

### Assumptions Made:
- Coefficients are stored in a list where the index represents the power of x.
- Operations include addition, subtraction, multiplication, and more.

### Test Cases:
- Define two polynomials and test addition.
- Test edge cases such as empty coefficients or unequal lengths.

```python
class Polynomial:
    def __init__(self, coefficients):
        self.coefficients = coefficients

    def __repr__(self):
        return " + ".join(f"{coef}x^{i}" for i, coef in enumerate(self.coefficients) if coef != 0)

class PolynomialOperation:
    @staticmethod
    def add(p1, p2):
        length = max(len(p1.coefficients), len(p2.coefficients))
        result = [0] * length

        for i in range(len(p1.coefficients)):
            result[i] += p1.coefficients[i]
        for i in range(len(p2.coefficients)):
            result[i] += p2.coefficients[i]
        
        return Polynomial(result)

# Test cases
poly1 = Polynomial([3, -4, 2])  # Represents 3 - 4x + 2x^2
poly2 = Polynomial([1, 2, 1])   # Represents 1 + 2x + x^2
result_add = PolynomialOperation.add(poly1, poly2)
print("Addition result:", result_add)  # Expected: Polynomial representation of added result
```
