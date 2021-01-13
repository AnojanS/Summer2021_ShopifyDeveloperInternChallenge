
# PegExchange

How to Install: `npm install` <br>
Running locally: `npm start` <br>
Run Tests: `npm test` <br>
Example Calls: Refer to the Tests :). There is also a Postman file included in this repository. This includes a test hook which will automatically set your authentication token, however you may need to manually change some of the id's in the calls.

The config folder is included for demo purposes.

### Overview
This is a photo exchange platform where users can upvote their favourite photos by spending virtual credits. Each user starts with 0 credits, and receives a credit for each image that they upload.

### Features
**Server Functions**
\* = no token required 
|  |  |
|--|--|
| User | - Create User* <br>- Login User* <br> - Logout user <br>- Delete self <br> &nbsp;&nbsp;&nbsp; - Cascade delete all user images |
|Image| - Upload images/ Batch Upload <br>&nbsp;&nbsp;&nbsp; - Filter files to only allow jpg and png files smaller than 16MB <br> - Get My Images <br>&nbsp;&nbsp;&nbsp; - Toggle public / private only <br>- Toggle Image Privacy <br> - Delete image <br> - Delete all images
|Upvote|- Upvote an image <br>&nbsp;&nbsp;&nbsp; -Increments uploader credits, decrements upvoter credits (only if upvoter has sufficient credits)<br> 	- Remove Upvote from an Image <br>&nbsp;&nbsp;&nbsp; -Returns credits to sources. User credits will not dip below 0|

**Security / Auth**
- Tokenization
	- Tokens are used to track user session and to authenticate the user behind every call. Aside from creating and logging in, every request requires a token. CRUD operations validate that the resource is being modified only by users who have the permissions (the creator).
- Password Hashing
	- Passwords are encrypted using bcrypt and a shared secret


## Approach
The most important constraint in this scenario was time. As a result, I chose to prioritize development speed by working with technologies that would allow me to get up and running quickly. I also wanted to avoid over-designing this application, so I designed it for the use case that I had in mind. If the use case changes, the design can always be revisited. 

## Technology Choices
| Purpose | Name | Reason|
|--|--|--|
|Language  | Javascript | I had some light experience building servers with Javascript, so I wanted to stick with a language that would allow me to get up and running as soon as possible |
|Runtime  | Node.js | Node's event loop improves speed and scalability by allowing for non-blocking I/O operations. <br> NPM has an extremely rich package ecosystem - Why reinvent the wheel?
|Database| MongoDB Atlas| I chose to use a non-relational database, as it would allow me to connect to a database without spending time writing SQL. This allowed for very straightforward database read and write operations.

### NPM Packages
| Purpose | Name | Reason|
|--|--|--|
|Server  | Express | Well documented package that allowed me to quickly configure a web server |
|Database Wrapper  | Mongoose | MongoDB wrapper that provide hooks|
|Image Uploads  | Multer | Processing and applying rules for image file uploads |
|Image Conversion  | Sharp | Converting images to Buffers for storage |
|Password Hashing  | bcrypt | Hashing passwords so that they're not stored as plaintext |
|Tokenization  | jsonwebtoken | Used for authentication and login/logout|
|Testing  | Jest | Testing library |
|Testing  | Supertest | Makes requests to the server when testing |
|Development| Nodemon | Hot reloading when developing |

### Technology Tradeoffs
- I used mongoDB's cloud offering, Atlas, in order to avoid consuming resources while developing locally. At a larger scale, this would affect latency.
- Storing the image as buffers imposes some restrictions on the file size depending on the CPU architecture that is running the application. I made this decision in order to quickly and efficiently store images. At a larger scale, I would use bucket storage or a CDN in order to store and serve images rather than storing them directly in the database.
- Javascript's dynamic typing can make it less robust than a compiled, statically typed language.

## Testing Strategies
- Most endpoints are individually tested with a "good" and "bad" flow
- Tests are run sequentially, since they are sharing the same database
- Within each test suite, tests are run sequentially. This means that the tests may affect each other. 
- Some integration tests between modules


## Areas of Extension
|  |  |
|--|--|
|Code Improvements|- Add a layer of abstraction by creating a service layer between the API and Models. This would make testing easier, and would allow for more descriptive function names<br> - Test more edge cases <br> - More descriptive error codes |
|Architecture| - Store images outside of database to support larger files and improve speed |
|Features| - Allow users to sort photos by trending / popularity <br> - Allow users to add and search photo tags |
|Misc.| Improve documentation to make it easier to demo :) 
