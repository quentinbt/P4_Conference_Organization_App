App Engine application for the Udacity training course.

## Products
- [App Engine][1]

## Language
- [Python][2]

## APIs
- [Google Cloud Endpoints][3]

## Setup Instructions
1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
1. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][4].
1. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
1. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
1. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][5].)
1. (Optional) Generate your client library(ies) with [the endpoints tool][6].
1. Deploy your application.


[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool

---------------------------------------------------------------------------------

# Udacity project part

## 1 Add Sessions to a Conference

following endpoint methods has been added
- getConferenceSessions:
	Return list of sessions from the websafeConferenceKey.
	path https://url/sessions/{websafeConferenceKey}

- getConferenceSessionsByType:
	Return conference sessions filtered by typeOfSession.
	path https://url/sessions/{websafeConferenceKey}/type

- getSessionsBySpeaker:
	Return conference sessions filtered by speaker.
	path https://url/sessions/speaker

- createSession:
	Create new session using _createSessionObject().
	path https://url/session

- _createSessionObject()
	this function will check the input and change if needed (for exemple it will transform a string in date) and then create the new session.

Session class and SessionForm has been define like so:

    name = ndb.StringProperty(required=True)
    highlights = ndb.StringProperty()
    duration = ndb.IntegerProperty()
    typeOfSession = ndb.StringProperty()
    date = ndb.DateProperty()
    startTime = ndb.TimeProperty()
    speaker = ndb.StringProperty()

Session properties is implemented in the same way than Conference is. This allow to maintain proper formatting inside the API. I decided than everything will be StringProperty except for:
 - duration: in this case working with integer is more easier than string
 - date: is a DateProperty for the same reason than duration
 - startTime: is a TimeProperty because it allow startTime to be measured in 24 hour notation.


Session entity is as a child of the Conference entity and speaker is a String in the Session entity. When you create a new session the task set_featured_speaker will copy and put the speaker and the session name in the memcache.

User can also add/delete session to their wishlist using the websafeSessionKey, this key is just added to the user profile, this is implemented like so:
	sessionKeysWishList = ndb.StringProperty(repeated=True)

creation flow:

	you create an new conference.

	└── using the websafeConferenceKey you can create a new session, this session will be assign to the previous conference.

		├── speaker/session name is automatically put in memcache.

		└── using the websafeSessionKey you can add/delete a session from you session wishlist.

## 2 Add Sessions to User Wishlist

following endpoint methods has been added
- addSessionToWishlist:
	Add a session to the user's wishlist using websafeSessionKey.
	path https://url/wishlist/{websafeSessionKey}

- getSessionsInWishlist:
	Get session in user's wishlist.
	path https://url/wishlist

- deleteSessionInWishlist:
	Delete Session from user's wishlist using websafeSessionKey.
	path https://url/wishlist

## 3 Work on indexes and queries
- indexes has been created

- 2 queries has been added
	- getSessionsByHighlights:
		Return requested conference sessions by highlight.
		path https://url/sessions/highlights

	- getSessionsByDuration:
		Return requested conference sessions by duration.
		path https://url/sessions/duration

- Answer: The datastore doesn't support multiple fields, this query will return "BadRequestError", A way to bypass this limitation is to use 2 differents queries, the first one will filter by startTime (in this case before 7pm), the second one will filter by typeOfSession (in this case non-workshop) and then we will look for the intersetion of these queries and output this intersetion.


## 4 Add a Task

- App Engine's Task Queue has been added

- method getFeaturedSpeaker() methods has been added