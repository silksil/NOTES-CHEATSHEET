In order to determine whether someone is being logged-in or has the authentication,  we create an AuthReducer that can return three things:

| Situation   | AuthReducer | Returns  |   
|-------------|-------------|----------|
|  Make request to bakckend to get current user           |    null         |   'null' indicates we really don't know whats up right now (e.g. when request is still pending)       |
|  Request complete, user is logged in          |     User model        |    Object containing user ID      |
|     Request done, user *is not* logged in        |      false       |   Flase means 'yep, we are sure the user isn't logged in'       |
