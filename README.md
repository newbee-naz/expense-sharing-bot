# expense-sharing-bot

SplitMate – Expense Sharing Application
=======================================

✅ Steps to Run the Application
-------------------------------

1. Clone & Build Project
-------------------------
git clone <your-repo-url>
cd expense-sharing-app
mvn clean install


2. Run Application
------------------
Run using Maven:
mvn spring-boot:run

Or directly from IDE:
- Run SplitMateApplication class → Run as Java Application

If successful, you’ll see logs like:
Tomcat started on port(s): 8080
Started ExpenseSharingApp in 4.567 seconds


3. Login via Google
-------------------
Open browser and visit:

http://localhost:8080/oauth2/authorization/google
http://localhost:8080/api/whoami


4. Access H2 Database
----------------------
H2 Console URL:
http://localhost:8080/h2-console

Connection details:
- JDBC URL: jdbc:h2:mem:expshare
- User: sa
- Password: (leave blank)

Note: Dummy data is auto-generated in the H2 DB (tables: User, Group, Expense, Settlement).


5. Swagger API Docs
-------------------
Since OAuth2 is handled in the browser, you can:
- Use the session cookie from browser
- Or directly test APIs via Swagger UI

Swagger URL:
http://localhost:8080/swagger-ui/index.html#/


6. Example API Workflow
------------------------

Create a Group
POST /api/groups
{
  "name": "Trip to Dubai",
  "members": ["user1@gmail.com", "user2@gmail.com"]
}

Add an Expense
POST /api/expenses
{
  "groupId": 1,
  "amount": 100,
  "description": "Dinner",
  "paidBy": "user1@gmail.com",
  "splitType": "EQUAL"
}
→ Updates balances automatically.

Settle Balance
POST /api/settlements
{
  "payerId": 2,
  "receiverId": 1,
  "amount": 25
}

Recompute Balances
GET /api/groups/{id}/balances
→ Useful to recalculate balances from all transactions (prevents corruption).


7. Shut Down
-------------
Stop application with:
CTRL + C


====================================
List of APIs
====================================

GET /api/groups/{groupId}/members → List all members of a group with balances

DELETE /api/groups/{groupId}/members/{userId} → Remove a member from a group (owner or self)

DELETE /api/groups/{groupId}/leave → Leave a group (with balance & ownership checks)

POST /api/groups → Create a new group (adds creator as owner + member)

POST /api/groups/{groupId}/members/{userId} → Add a member to a group (only owner allowed)

GET /api/groups → List all groups the current user belongs to

POST /api/expenses → Add a new expense to a group (splits automatically, supports custom split)

GET /api/expenses/group/{groupId} → List all expenses for a specific group

POST /api/settlements → Make a settlement between two users in a group (payer pays receiver, transactional update of balances)
