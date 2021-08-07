# Eckmar-Script-V2
The infamous half baked and insecurities Market Soltion ,Eckmar Script v2 run on laravel 5.4 ,a script that costs around 600 dollars 


About script

Marketplace v2.0 is also written in PHP with Laravel framework. Its using latest standards for encryption (like Sodium library) and security. Its made without use of JavaScript, so its optimized for TOR, but can also be ran normally like any other web app (as you can see on demo).

Requirements

    VPS with at least 2GB of RAM
    Daemon for each coin that is enabled on marketplace


Server requirements: (what software is needed on VPS)

    PHP 7 (recommended and tested on 7.2)
    SQL Database (MySQL,PostgreSQL, SQLite, SQL Server)
    Elasticsearch (Search interface that will keep track of search records and provide great search performance)
    Redis (Optional, but will greatly increase app performance )


Features

Categories
Category system is very dynamic. Categories can be nested indefinitely. Creating, deleting and editing of categories is handled in admin panel.

Detailed home page
There is placeholder text on the home page at the moment that will represent most important features of your marketplace.
On top of that, all users can see Top Vendors (Vendors with most sales), Latest Orders (Products of orders completed most recently, as well as order value, but no information about buyer or seller), Rising Vendors (Vendors with most sales in specified time frame, defaults to 7 days).
Official mirrors is place where you can specify on what other links your website can be reached.

Password reset
Password can be reseted with PGP Key added on account (more about PGP below) or with Mnemonic key provided during signup. Mnemonic key is shown only once and should be written down on paper. During signup it is hashed (bcrypt) instantly and only stored in operating memory for the short time during display after which is cleared from memory manually as addition to automatic PHP Garbage Collection.

PGP
Each user can add their on PGP key which is used for multiple features. Vendors must have PGP and 2FA enabled before they are allowed to upgrade from user to vendor status. If you have active PGP key you can reset your password with it or enable 2FA for your login. Each PGP key must be confirmed before is linked to account, and if you want to add new key you must first sign a message from the old one.
Messages can also be encrypted with user's PGP key if its present (this is not enforced, its user's choice).

2FA (Two-Factor Authentication)

If user has PGP key linked to their account, they can enable 2FA. If enabled, login to marketplace will be prevented unless randomly generated message is signed from the key.

Wishlist

Buyers do not need to save product links for later. On each product there is "Add to wishlist" button that can be used, and they are stored in user's personal list for later.

Vendors

Normal users cannot post products on Marketplace. In order to post products you must become vendor. Before you can upgrade you must have PGP key and 2FA Enabled in your profile. Vendor price can be set in marketplace config. Vendor can use any of the coins available to purchase vendor status. Out of each vendor sale, a percentage of sale value goes to marketplace addresses.

Vendor profile customization

Vendors are able to customize their profile with pre-defined profile backgrounds and short personal description.

Vendor levels and XP

Each new vendor starts at Level 0 and progresses further based on performance. XP and Amount of levels is dynamic and configurable. Multipliers determine how much XP is granted to/taken from vendors for each action. XP is granted/taken by formula: USDvalue*multiplier
Example:
     product_delivered multiplier is 20
     USD value of product is 100$
     When vendor successfully delivers product, he will receive 100*20=2000 XP
     This is example from experience config file:
Code:

    'multipliers' => [
        'product_delivered' => 10,
        'product_dispute_lost' =>20,
        // How much XP per star (given/taken based on feedback type)
        'feedback_per_star' => 2,
        // how much XP per USD value of transaction (given/taken based on feedback type)
        'feedback_per_usd' => 5,
    ]


Feedback

After each completed purchase, vendors are able to leave feedback. Feedback types are Positive, Neutral and Negative and they will affect vendor score as well as product score.

Multiple Coins

Probably the most important system is Coin System. Its completely dynamic, which means new coins can be added at any time. Standard version of marketplace comes with Bitcoin and Monero included. For each coin added, in marketplace config there can be set unlimited amount of marketplace addresses (used for receiving fees from purchases), and in case more than one address is present, address for receiving fee will be choosen randomly (for each purchase).

Product types
There are two product types. Physical and Digital products. Based on the type, different options are displayed during product creating and purchase.
Both Digital and Physical products support offers and custom units of measure (Item, kg, gram, piece ...). With offers, vendors can give discounts on purchase based on quantity. For example:
Price for 1 product is 100$
Price for 10 products is 90$
Price for 20+ products is 80$
Each of those is considered an offer and can be added/removed at any time.

Digital products support automatic delivery which is optional. If checked, autofill system is used. Each line in textarea is treated as single item and will be product's quantity. It will be instantly sent on user upon purchase.

Physical products have delivery options. Each delivery option consists of: Name, Price, Expected delivery duration, Minimum quantity for delivery, Maximum Quantity for delivery. Physical products can also include/exclude countries from shipping.

Markdown styling
Instead of just plain text, product description and rules support markdown styling. Every tag is supported except URL tag.

Purchasing

When user chooses to purchase any product, he is able to pay with any coin supported by market (and vendor, since vendors can choose which coins they want on each product). There is no wallets or anything similar. Users do not need to keep money on marketplace at all times. For each purchase random address is generated, and its used for that purchase only.

Escrow

Escrow is present on every purchase by default. Upon purchase, marketplace address is generated that will hold funds. If purchase is completed if its marked as delivered or dispute is resolved. If buyer is unhappy with purchase he can open dispute and potentially (based on admin's decision) get his money back. Upon purchase completion, based on result money will be sent from temporary purchase address to buyer/vendor and to one of the marketplace holding addresses.

Cart

If user wants to buy more than one product (maybe from different sellers too), they can add them all in a cart and then checkout only once.

Messages

Most important feature of messages is security. Marketplace uses latest algorithms and standards in Public Key Cryptography (like XChaCha20-Poly1305-IETF) to encrypt messages between users. Upon registration, Public and Private keys are created for each user. Based on user's password an encryption key is derived, and that key is used to encrypt Private key, while Public Key is exposed. When user A whats to send message to user B, a key exchange happens. User A encrypts message with User B's public key, and that message is stored in database. Only user B can read that message when he logs in and decrypts his messages with password. This system makes messages secure and unreadable by anyone, including marketplace administrator or basically anyone who can possibly get access to the database.

Messages are organized in conversations. Multiple conversations can be started at the same time.

Notifications

Users will get notifications for most actions that happen on marketplace regarding them. Some of the examples are: New message, Purchase status update (product sent, product delivered etc.), Vendor actions (Feedback) and so on.
They can be read in User Account Panel and deleted at any time.

Bitmessage

Marketplace can possibly connect to Bitmessage daemon. If connected, users can chose to add their Bitmessage addresses and get their notifications forwarded there. This means they will still get notifications even if they are not currently logged in, and they don't need to refresh anything.
Before being able to forward notifications, Bitmessage addresses must be confirmed first.

JavaScript Warning

Optional warning can be enabled in marketplace config. If visitor has JavaScript enabled, a message will be displayed notifying them about security issues.

Support

Users can open support tickets regarding any problem they encounter. Administrators/Moderators will see this tickets in admin panel and can reply, or close them.

Admin Panel

Most of the stuff happening on marketplace can be viewed directly on admin panel. Administrators can access every feature on admin panel.

Moderators
Modular permission system is currently supported, which means admins can give/take some access to moderators (For example, support staff can only answer tickets and resolve disputes, community manager can only send mass messages etc.). Currently supported features:

    Index - Basic information
    Categories - Add/Edit/Remove Categories
    Mass Messages - Ability to send messages to users by marketplace (Can be filtered to user groups)
    Users - View users, search, filter, and edit each user individually.
    Products - View, search, filter by user, or edit product
    Log - Activity log of all Administrators/Moderators inside Admin Panel Example: 

Code:

User: eckmar Type: change Description: Administrator status taken from user Performed on: exampleUser123 Date: 2019-03-20 11:36:52

    Bitmessage - Status of Bitmessage service (performs test), and view of marketplace bitmessage address
    Disputes - View and resolve purchase disputes
    Tickets - View and resolve support tickets
    Purchases - List of all purchases
    Vendor Purchases - List of vendor purchases 


Supported coins

Marketplace currently supports these coins:

    Bitcoin - Included in standard version
    Monero - Included in standard version
    Litecoin
    DASH
    PIVX
    Verge
    Bitcoin Cash


Installation

Marketplace installation instructions are included. These are not 100% copy paste but they do explain how must of the things work in detail.
