# Find-Sparks Dating App - back-end

[Dating site](https://findsparks.uk/) built to learn and demonstrate ability to code more complex applications. Seen front-end [here]https://github.com/ArdalanJaf/find-sparks).

## Software

- Javascript
- React
- Redux
- Axios
- node.js
- Express
- nodemailer
- joi 
- SQL
- Bootstrap
- SASS
- API (api.postcodes.io)

## Features

- Full onboarding, including validation and obtaining profile picture with react-webcam.
- Matching algorithm (made with pure JS) that first filters, then sorts, members for review (like/pass) by user.
- Online messenger for matched members to talk to eachother.
- SQL database to store all users data.
- Token-based middleware security.
- Uses nodemailer to automatically email new users (with structure to add multiple languages).
- Postcodes API (called through Axios) to determine distances between users.

## Method

1. User registers, providing their information, dating preferences and an image of themselves. Input is validated through joi before being submited.
2. User data is compiled into an object, which includes a randomly generated user_id. User_id is the main anchor for refering to users, respectively.
    - User data is stored in SQL, to be pulled when required.
3. User can then review other users in matching. Upon loading said page, an algorithm creates a copy of the entire users object - with incompatible users filtered out and sorted with most compatible users first.
    - Filtering: A number of filters remove members who concretely do not match user's criteria (and vice-versa). 
      - E.g. member lives beyond user's stated minimum distance.
    - Sorting: Applies points based on compatibility for flexible criteria, and organises list with most-compatible first.
      - E.g. If user is "unsure about kids", a member that "wants kids in the future" will get more points than one that "wants kids ASAP".
4. User has option to "like" or "pass" each member presented for review. In either case, member is added to user's "seen list" (inside their user-object) to ensure they won't be displayed again. If "liked", the member's user_id is added to the user's "like list" (also inside their user-object).
5.  If user likes a member that has liked them (or member later likes them), a "conversation" begins in the "messages" section allowing said users to talk to eachother. 
    - Upon loading "messages", all user_ids' in the user's "like list" are cross-examined to see the user's user_id is also in their "like list", respectively. If so, a conversation is created. 
 
 ### Data
 
 - User data is stored in SQL format through the back-end. 
 - Master users-object (containing all user-objects) is downloaded (and periodically re-downloaded) to front-end upon log-in or registering[^1].
 - Changing user information (such as a new member being added to their "seen list") immediately communicates with the back-end to update the SQL database. 

### Security

- Upon registering or loging in, a token is generated for a particular user. This token is required for communication between the front-end and back-end (except registering and loging-in). When a user logs-out or logs-in from somewhere else, a new token is generated.

[^1]: This is due to the front-end being built first, and the back-end added later. If there were no time-restraints, all functionality that required all users data would be moved to the back-end. 
