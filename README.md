**A DIGITAL BUSINESS ANALYSIS AND PROBLEM-SOLVING CASE STUDY OF
SWIGGY**

**SplitPlate (group order & bill-split with calorie tracker) and a dineout vibe checker**

BY:
Aanya Agarwal - 2533301
Agastya Pallavi Shethia - 2533304
Akankshya Pradhan - 2533305
Upanshu Sil - 2533358
Vibhi Maheshwari - 2533361

**1. Organization Overview**

**1.1 Nature of Business**
Swiggy is one of India's leading hyperlocal, on-demand digital commerce companies, headquartered
in Bengaluru. The startup was founded in August 2014 by Sriharsha Majety, Nandan Reddy and
Rahul Jaimini and started its operations in Bengaluru as a food delivery service. The company
currently has operations in over 700 cities across India and is also running quick commerce grocery
delivery service, Instamart, in over 100 cities. In November 2024, Swiggy went public with an initial
valuation of $11.3 billion. The initial listing price for Swiggy was Rs. 4,800 crore, which represents a
rough valuation of $11.3 billion. Swiggy also offers Dineout (restaurant bookings) and SteppinOut
(event bookings) in addition to food delivery and Instamart. Swiggy, which had earlier launched its
parcel delivery service (Genie), discontinued the same in May 2025 as it concentrated on its two
main segments—food delivery and restaurant table booking. This report Centre Island focuses on
Swiggy's main verticals – food delivery and Dineout – as the three digital solutions that the group
offers – SplitPlate, embedded calorie tracker, and Dineout Vibe Checker (detailed in Section 6) – are
squarely in the food ordering and restaurant discovery experience.
**1.2 Business Model**
Swiggy is a multi-sided (marketplace) platform that brings together both customers and restaurant
partners (including delivery partners (gig workers)) without producing or selling any food. It makes
money because it helps these groups to transact with each other. It generates its main revenues from:
• Commission paid to restaurant partners depending on the value of each order (as a
percentage).
• Direct delivery charges and small platform/packaging charges.
• Restaurants seeking improved visibility in search and category pages will receive advertising
and promoted-listing revenue.
• Subscription fees for the “Swiggy One” membership program that provides free or
discounted deliveries and other benefits.
• Revenue from ancillary business such as Instamart (food delivery) using the same deliverypartner network.
This model can be scaled to accommodate a large network; Swiggy reportedly has more than 100
million registered users and has tied up with more than 190,000–200,000 restaurants and averages
over 450,000 delivery partners per month. Zomato (earlier Eternal), which has a bigger stake in
India's online food delivery market, is its main competitor in the food delivery business, while
Instamart is a rival to quick-commerce service Zepto and Blinkit, which is the fastest growing
segment of the online food delivery market. Even though Swiggy has been making steady revenue
growth, the company has been on the net loss side since inception, and factors such as order density
and average order value (AOV), which could impact from SplitPlate and calorie tracker could be key
levers for its profitability. With that type of asset light business, profitability relies on order density,
as well as repeat use — something our features would help with, whether it's making a group order
frictionless (SplitPlate) or helping guests determine if the restaurant's vibe and crowd is what they're
looking for (the Vibe Checker).
**1.3 Digital Business Ecosystem**
Each of these systems is interconnected and must synchronize in close real-time to complete an
order. These are all interdependent systems, from the customer facing side, to the partner facing side,
to the backend systems that must synchronize – almost in real-time – to complete an order. This
ecosystem would be represented as shown below with the addition of the SplitPlate and Dineout
Vibe Checker.
Figure 1: Swiggy's digital business ecosystem, including the proposed SplitPlate module and
Dineout Vibe Checker.
The Swiggy core platform sits at the centre and is responsible for order management, dynamic
pricing, and partner matching. Encircled by it are the customer app, the restaurant partner app, the
delivery partner app, payment gateways/UPI rails, an analytics/data-warehouse layer, and third-party
services like maps and SMS providers. Our two features fit into this ecosystem, with SplitPlate
sitting between the customer app and payment gateway, creating multiple individual payment
requests as opposed to one individual payment, and the Vibe Checker sitting between each Dineout
partner restaurant and the customer app, pushing live crowd and ambience updates directly into the
listing of each restaurant.
**1.4 Target Customers**
Swiggy's customer base spans over 100 million registered users across more than 700 cities, but
several segments are especially relevant to the two features this group has chosen to design:
Working professionals and students living in cities who order food often and regularly use
roommates or teammates to eat with, often find that ordering in groups and splitting the bill is a
hassle.
Single orders are when splitPlate tags a cart for a group or family as a single order, thereby
eliminating the need for "who ordered what" manual work.
Users who want to stay healthy and fit, but do not track their daily calorie intake and are not
currently aware of the impact on their diet from a food delivery order.
Those customers who find they can save more money by sharing the price of their delivery and
platform rather than paying for an order one at a time.
Vibe Checker's live updates benefit directly socially-driven diners who are as interested in a
restaurant's atmosphere as its food, which is a population that is definitely part of Gen Z and
currently has no way to determine the vibe or crowd at a restaurant before they book.

**2. Information and Decision-Making**

**2.1 Data, Information, Knowledge**
It can be understood through the lens of data, information and knowledge as each of these factors
provides its context and helps Swiggy transform the raw data into effective data that help them make
good business decisions.
Data: Facts that are not processed, meaning that they don't have any meaning — e.g. the number 240
against a menu item, a GPS coordinate, or a timestamp of the order-button click.
Information: Data that is put in an arrangement that makes it meaningful — e.g. "Chicken Biryani
was ordered 4,500 times this week in Bengaluru.
Knowledge: Information is interpreted and understood in context, patterns emerge, e.g. on weekend
evenings and rainy days, biryani orders are higher.
The value created at every level in each of our three can be summarised as follows: our SplitPlate
and calorie tracker transforms per-item cart data into personalised calorie information, which is
knowledge about eating behaviours; our Vibe Checker transforms live crowd and ambience logs into
knowledge about which places fit which moods and times, which management can act on wisely
**2.2 Types of Data Generated by the Organization**
There are several kinds of data that are generated by the organization.
Transactional data—order records, item prices, payment confirmations, per person split amounts and
settlement status.
Behavioural data: menu browsing patterns, time on a restaurant page, cart abandonment.
Geolocation & logistics data: customer address, restaurant location, gps pings of delivery-partner,
eta calculation.
Unstructured text data: ratings and reviews, and Vibe Checker ambience notes and crowd-level tags
submitted from each partner restaurant.
Nutritional information: Calorie content per food item, and per person per meal
Partner-side data: Menu changes, kitchen preparation time, partner's performance metrics and many
more.
**2.3 How Information Quality Affects Managerial Decisions**
The value of an information system is determined by the quality of information provided by the
system, which are measured by accuracy, timeliness, completeness, relevance, and consistency. If
there are inaccuracies in the calorie countings, then the tracker would lose its credibility. Even if
crowd/ambience info is updated periodically, but not consistently, it would leave a listing's
crowd/ambience status outdated right as a diner is picking a restaurant. On the other hand, if the
number of available delivery partners is counted accurately and in real-time, dispatch algorithms can
match the partner to the nearest one and deliver goods in real-time, which decreases delivery time
and cost.
**2.4 Operational and Strategic Decisions Based on Information Systems
Operational decisions (day-to-day):**
• In real time, assigning the closest delivery partner to a new order.
• Recalculating each SplitPlate's percentage of the group when items are added or subtracted.
• The Vibe Checker who determines, visit by visit, what type of listing the current crowd level
and ambience is.
• Abating the restaurant's order line when the cooking time is running late.
Strategic decisions which are made at a a longer time horizon:
• SplitPlate is launched only after analysing demand signals such as the frequency of
manually-split orders.
• Coordinating expansion of delivery partner capacity in specific cities and regions, based on
demand forecasting models.
• Increasing live vibe update needs in more cities will call for investing in Vibe Checker
staffing and shift coverage.
• Seeking opportunities to partner for healthier menus or health-focused marketing by using
anonymised calorie information.

**3. Business Information Systems Analysis**

Swiggy's technology stack follows the typical structure of an organizational information system,
from operational systems handling individual transactions to systems designed to support top
management strategy.
**3.1 Transaction Processing Systems (TPS)**
The TPS records every transaction the moment it happens: an order placed, a payment authorised, a
delivery-partner status update, or every individual UPI request SplitPlate generates as a member
pays the organiser back. TPS data must be accurate and real time, since every further system is based
on that data.
**3.2 Management Information Systems (MIS)**
MIS present TPS data in a structured format to middle management: sales by city dashboards,
delivery-partner performance reports, and a dashboard of Vibe Checker submissions by restaurant
and time slot.
**3.3 Decision Support Systems (DSS)**
DSS are used for semi-structured ‘what if' decisions rather than for reporting what has already
happened; for example, a demand-forecasting model, a dynamic-pricing model and a ‘what if' model
that could suggest a restaurant's required number of Vibe Checker shifts according to historical
footfall and booking trends.
**3.4 Enterprise Systems**
Enterprise Systems connect data throughout the organisation, from an ERP for finance and HR, to a
CRM system that tracked a customer's history of Dineout booking, and their vibe preferences so that
future suggestions remain relevant, to logistics/SCM systems that linked restaurant capacity to the
delivery fleet.
**3.5 E-Business Platforms**
These are the digital storefronts that the above systems are accessed through: the customer app and
website, partner dashboard, delivery-partner app and APIs connecting Swiggy to Maps, SMS
providers (Razorpay, GPay, PhonePe), and payment gateways, which SplitPlate depends on for
individual payment requests.
**3.6 How These Systems Interact**
These five layers are not separate, but rather they are part of a continuous data pipeline, as illustrated
below, in that each layer is both a producer and a consumer of the data by the other layers.
Figure 2: Interaction between the e-business platform, TPS, MIS, DSS, and Enterprise Systems at
Swiggy.
Each transaction starts in the e-business platform (app), gets recorded in the TPS and is then rolled
up to higher-level MIS reports and Enterprise Systems records. The management then uses the DSS
models to make forecasts and recommendations, based on the summarised MIS data and other data
like weather and festival calendars, for routine and strategic decisions. Those changes trickle back
down as configuration or policy changes (new delivery-fee regulations, new Vibe Checker shift
schedules, new menu items for stocking).

**4. Digital Business Workflow Analysis**
   
This section is not only the ordering-to-discovery workflow of the calorie tracker SplitPlate, but also
the Dineout Vibe Checker touch.
**4.1 Order Placement to Delivery (Business Process Map)**
The journey starts before checkout, as the organiser must add group members and tag items before
the cart is completed. This journey is illustrated in the process map below.
Figure 3: Business process map from group cart creation through to delivery and post-delivery
support.
A unique quality to our SplitPlate and calorie-tracker features shown in this map at step 1-2:
Tagging an item to a person simultaneously calculates their monetary share and running calorie total,
so the tracker needs no separate data entry. The Vibe Checker operates on Swiggy's separate Dineout
journey rather than this food-delivery flow, and is covered in Section 4.3.
**4.2 Payment Processing (SplitPlate Data Flow)**
The most difficult aspect of SplitPlate is the ability to bill-split groups, where multiple payments are
made against one order. The diagram below shows this data flow.
Figure 4: SplitPlate payment-processing data flow, from cart-splitting to individual settlement.
The organiser always pays the full amount to Swiggy upfront, as only one of them pays Swiggy per
order. The SplitPlate then creates separate UPI requests for each member, monitors each member's
status separately and dynamically updates the organiser's pending value. This means that the TPS is
simple and that it is layered on top of a peer-to-peer settlement system.
**4.3 Restaurant Discovery Workflow (Dineout Vibe Checker)**
This will give restaurant discovery on Dineout a human touch. A companion app is used by a parttime Vibe Checker stationed at each restaurant or cafe to record the current ambience of the
restaurant (lively, chill, date-night) and the level of the crowd (quiet, moderate or packed) at a
specific restaurant café at a specific time. As a customer walks down the street and shops at one of
the restaurants in the vicinity, the listing for that restaurant in Dineout changes in near real time, not
just based on its menu or star rating.
Figure 5: Dineout Vibe Checker workflow, from on-site observation to a live listing update and
booking.
All updates are recorded back to the MIS/DSS layer described in Section 3, which enables
Operations Managers to view shift coverage trends by restaurant as well as view vibe and footfall
trends and make adjustments to Vibe Checker shift coverage as needed
**4.4 Inventory and Menu Updates**
The restaurant partners update the availability and price of the items on the partner dashboard in near
real time and the calorie tracker fetches (or guesses) the calorie information from this same source.

**5. Strategic Advantage Through Digital Systems**
   
The following lists the various ways in which digital systems are driving competitive advantage at
Swiggy.
**5.1 Competitive Advantage**
The practice of splitting bills in sections is rare on Indian food apps, and an integrated calorie tracker
is like a niche set that isn't easy to duplicate. A human verified crowd along with an ambience layer
for Dineout is also challenging to reproduce without establishing the same on-ground network.
**5.2 Customer Retention**
The friction of “who owes what” is eliminated and there is a tangible reason for customers not to
want to order in groups. Health-conscious users will be loyal to a platform that understands their
goals, and diners who consistently find a restaurant that vibes with them will be more likely to return
to Dineout.
**5.3 Automation**
SplitPlate does the fee-splitting and payment tracking automatically. The Vibe Checker network
takes care of the ambience and crowd data, which would have to be scraped in other ways or be
guessed.
**5.4 Scalability**
SplitPlate's per-order calculations scale linearly without requiring any new infrastructure. Along
with this, the Vibe Checker model scales with an addition of more part-time checkers city-wise,
following the same gig-economy playbook Swiggy has for delivery partners.
**5.5 Personalization**
The calorie tracker transforms a standard order into a personal health record for each person while
the Vibe Checker allows Dineout to customise restaurant recommendations based not only on the
star ratings but on what the restaurant has been like in the past to the people who dine there.
**5.6 Operational Efficiency**
With the use of live vibe data, there is less guesswork involved, and fewer walkouts or no shows
because of booking a restaurant blind. SplitPlate eliminates payment collection friction, which leaves
the organiser out of the platform and chasing its group members.

**6. Challenges and Recommendations**

The group first identified three specific areas of Swiggy's app experience which were lacking for
them and then suggested some novel digital features. These are expressed as a problem and the
solution this report proposes below.
**Problem 1** — No way to split a group order for the users native to Swiggy as of now: If multiple
people make an order together on Swiggy today, there is no in-app mechanism to tag the order with
who ordered which and split the bill. Currently, the members of a group have to figure out this by
themselves and decide to order outside the platform completely (cash or an UPI transfer, which
Swiggy can't see), thereby dissuading them from ordering at all in the first place.
Proposed solution: An in-app module (SplitPlate) to facilitate group carts and bill splitting with an
organiser adding people to the group, tagging each item to a person, automatically splitting it and
collecting the payment (Sections 4.1–4.2).
**Problem 2** — No visibility into calorie or nutrition content: Without calorie or nutrition content
available anywhere in the ordering flow, health-conscious customers have no easy way to track their
consumption patterns and aim for better choices.
Proposed solution: The calorie tracker is built into the common cart that SplitPlate shares, so that a
running total of calories consumed per person can be calculated automatically as items are tagged
without needing to enter any additional data (Section 4.1).
**Problem 3** — The inability of restaurant listings to convey the vibe or crowd level on a particular
evening: Swiggy Dineout lists restaurant menus, ratings and a gallery of images, but not what the
restaurant feels like on a certain evening on a particular evening like how busy it is, or what mood
it's in, so that customers, especially Gen Z users who are booking a restaurant based on the vibe
more than the food, are left guessing when they arrive.
Proposed solution: A part-time onsite role of a ‘Vibe Checker' at every restaurant or café, partnered
with Dineout in Swiggy will allow students and others looking for part-time work a valuable
opportunity. The Vibe Checker loads the current number of people in the restaurant and the
ambience (be it quiet or lively or packed) into the restaurant's Dineout listing, allowing prospective
diners to get a real human read on the vibe before making their booking. This will also help Swiggy
get a new category of part-time employees along with its delivery staff.

**CONCLUSION**

Swiggy’s already existing infrastructure provides the base to implement features that are customer
friendly and improve the experience of consumers, which will also help improve business
financially. The proposed solutions—SplitPlate,, with an integrated calorie tracker and the Dineout
Vibe Checker, address gaps in the existing platform by simplifying group payments, promoting
healthier food choices, and providing real-time restaurant ambience updates. These features integrate
seamlessly with Swiggy's existing information systems, including TPS, MIS, DSS, and enterprise
platforms, without requiring major infrastructure changes. Through the usage of didgtal solutions to
solve problems customers face in the real world, Swiggy can strengthen customer loyalty, increase
engagement, and differentiate itself in India's highly competitive food delivery market.


flowchart TB

    SWIGGY["SWIGGY DIGITAL BUSINESS SYSTEM"]

    SWIGGY --> CUSTOMER
    SWIGGY --> RESTAURANT
    SWIGGY --> VIBE

    %% CUSTOMER BRANCH
    subgraph CUSTOMER["CUSTOMER"]
        C1["Swiggy App"]
        C2["SplitPlate"]
        C3["Calorie Tracker"]
        C1 --> C2
        C2 --> C3
    end

    %% RESTAURANT BRANCH
    subgraph RESTAURANT["RESTAURANT PARTNER"]
        R1["Restaurant Dashboard"]
        R2["Menu & Availability"]
        R3["Dineout Listing"]
        R1 --> R2
        R1 --> R3
    end

    %% VIBE CHECKER BRANCH
    subgraph VIBE["VIBE CHECKER"]
        V1["Vibe Checker App"]
        V2["Crowd Level"]
        V3["Ambience"]
        V1 --> V2
        V1 --> V3
    end

    %% CENTRAL SYSTEM
    C2 --> CORE
    R2 --> CORE
    R3 --> CORE
    V2 --> CORE
    V3 --> CORE

    CORE["SWIGGY CORE PLATFORM"]

    CORE --> TPS["Transaction Processing System"]
    CORE --> PAY["UPI / Payment Gateway"]
    CORE --> DB["Central Database"]

    DB --> MIS["MIS"]
    DB --> DSS["DSS"]

    MIS --> CORE
    DSS --> CORE

    
**References**

1. "About Us." Swiggy, Swiggy Limited, https://www.swiggy.com/about
2. IBEF. "E-commerce Industry in India." India Brand Equity Foundation,
https://www.ibef.org/industry/ecommerce
3. Statista. "Online Food Delivery Market in India." Statista, https://www.statista.com/
4. McKinsey & Company. The State of Grocery Retail and Digital Commerce. McKinsey &
Company, 2023, https://www.mckinsey.com/
5. Bureau, ET. "Swiggy IPO: India's Food Delivery Giant Makes Stock Market Debut." The
Economic Times, https://economictimes.indiatimes.com/
6. ETtech. "Quick Commerce Industry Unlikely to Sustain as Many Players as Today: Swiggy's
Sriharsha Majety." The Economic Times, 2026,
https://economictimes.indiatimes.com/tech/technology/ettech-interview-quick-commerceindustry-unlikely-to-sustain-as-many-players-as-today-swiggys-sriharshamajety/articleshow/131366643.cms
7. Reuters. "India's Swiggy Posts Narrower Q4 Loss with Strongest Food Delivery Growth in
Years." Reuters, 8 May 2026
8. The Times of India. "Swiggy Q4 Losses Widen as Instamart Slows Growth, Food Delivery
Jumps." The Times of India, May 2026
