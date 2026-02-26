
**FRONTEND TO BACKEND COMMUNICATION**<br>
•	Front end (HTML+JS) sends Http request using POST,GET,PUT and DELETE<br>
•	Backend (Flask) validates data, performs business logic, queries database and return JSON response.<br>
•	Front end updates UI dynamically<br>

**HIGH LEVEL API ENDPOINTS**<br>
•	Api/signup<br>
•	Api/verify OTP<br>
•	Api/Login<br>
•	Api/Foods<br>
•	Api/orders<br>
•	Api/cart<br>

**SYSTEM OVERVIEW**<br>
The system is a Food Ordering Backend system that manages:<br>
•	User Registration & Authentication<br>
•	OTP Verification<br>
•	Role-Based Access(User/Admin)<br>
•	Food Catalog Management<br>
•	Cart Management<br>
•	Order Procession<br>
•	Order Lifecycle Control<br>
•	Admin Operations<br>
It follows a Client to API to Database Architecture<br>


**FLOWCHART EXPLANATION**<br>
In here, highlights the various reasons why some decisions were made while designing the flowchart.<br>
<em>**1.	User Registration Flow**<em>: User enters email and password because unique identity is required which ensures traceability & communication, and lastly password secures authentication. Decision made: allows for either email/phone flexibility<br>
	Check Duplicate Email/Phone no: here, it is use too prevent account duplication, maintain the database integrity and avoid identity conflicts. 
Decision made: for data uniqueness enforcement<br>
	Valid Referral Code: In this flow logic, referral code may affect rewards/tracking and must prevent abuse with fake code. 
Decision made: Optional but validate if provided<br>
	Generate Otp: ensures user actually own the email thereby preventing fake accounts. Decision made: OTP expires after 5 mins to increase security<br>
	Store OTP + Expiry: Needed to verify later and prevents replay attacks. Decision made: Time bound verification.<br>
	User Submits OTP: This is done after system checks due to defensive programming, prevent logic bypass and as double verification<br>
	Activate Account: This is set to True why to prevent login before verification and enforce account authenticity<br>
<em>**2.	Login and Access Control Flow**<em>:
	User Submits Credential: for authentication request<br>
	Validate Credentials: to check if user exists and compare hashed passwords<br>
<em>**3.	Ordering Flow**<em>:
	User adds items to cart: to prevent manipulation and maintain integrity<br>
	User clicks “ Place Order “: to activate backend validation<br>
	Fetch Cart Items from Database: because front end can be manipulated<br>
	Validate Food Availability again: food may become unavailable after being added to cart. This prevents overselling and customer dissatisfaction<br>
	Recalculate Total Price from DB: to prevent price tampering because user may alter frontend price.<br>
	Check Payment Status: If unpaid, keeps requesting for payment to bypass that screen, hence business rule enforcement<br>
	Create Order Record: allows admin confirmation workflow<br>
	Clear Cart: Prevent duplicate orders<br> 
<em>**4.	Order Status Lifecyle Flow**<em>: To provide traceability, confusion, operational loss, logical corruption an as well as fraud<br>
<em>**5.	Admin Management Flow**<em>:<br>
	Add Food Item: to control product catalog<br>
	Update Price: Market changes and cost fluctuations<br>
	Mark Food Unavailable: ingredient shortage and temporary outage<br>
	Manage and update Order status: Kitchen confirmation required and delivery process monitoring<br>

**EDGE CASE HANDLING**<br>
The backend handles failures using ;<br>
a.	Input validation<br>
b.	Conditional checks<br>
c.	Lifecycle rules<br>
d.	Controlled responses and<br>
e.	Status codes<br>
It goes on to handles unexpected scenerios like:<br>
a.	Duplicate email/phone number<br>
b.	Invalid referral code<br>
c.	Invalid OTP<br>
d.	Expired OTP<br>
e.	Payment not completed<br>

**ASSUMPTION MADE DURING DESIGN**<br>
The assumption made during design were due to:<br>	
	User authentication using Email/phone number+ password<br>
	Passwords are hashed before storage <br>
	OTP is sent only via email<br>
	OTP validity is assumed to be 5 mins<br>
	No multiple authentication process beyond OTP<br>
	Customer cannot change order status<br>
	Cart is tied to a logged in user<br>
	No cart expiration logic specified<br>
	No refund workflow specified<br>
	No multiple payment method defined<br>
	No SMS integration provided<br>
	System assumed to be a single server<br>

**SCALABILITY THOUGHTS**<br>
The challenges will be:<br>
1.	Can it handle traffic?<br>
2.	Can it respond quickly?<br>
3.	Can it avoid crashing?<br>
**At Current State:-** <br>
i.	Single Flask App<br>
ii.	Single Server<br>
iii.	In memory Lists(simulation)<br>
iv.	SQLite database<br>
**At 10,000 Users Capacity:**<br>
i.	Move to production WSGI Server<br>
ii.	Using gunicorn app:app: to handle multiple workers, for better performance and support concurrent requests.<br>
iii.	Replace SQLite because they are for small projects and not large traffics, then upgrade to POSTgreSQL or MYSQL because of row level locking, better concurrency handling, connection pooling and indexing<br>
iv.	Add database indexing because it improves login speed, tracking speed and order lookup speed<br>
v.	We use multiple servers to distribute traffic and for high avaliability<br>

