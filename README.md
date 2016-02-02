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
	Return sessions.
	path https://url/sessions/{websafeConferenceKey}

- getConferenceSessionsByType:
	Return requested conference sessions by typeOfSession.
	path https://url/sessionsbytype/{websafeConferenceKey}

- getSessionsBySpeaker:
	Return requested conference sessions by speaker.
	path https://url/sessionsbyspeaker

- createSession:
	Create new session.
	path https://url/session

Session class and SessionForm has been define

## 2 Add Sessions to User Wishlist

following endpoint methods has been added
- addSessionToWishlist:
	Adds a session to a user's list of sessions.
	path https://url/wishlist/{websafeSessionKey}

- getSessionsInWishlist:
	Get user's wishlist.
	path https://url/wishlist

- deleteSessionInWishlist:
	Delete Session from user's wishlist.
	path https://url/deletefromwishlist

## 3 Work on indexes and queries
- indexes has been created

- 2 queries has been added
	- getSessionsByHighlights:
		Return requested conference sessions by highlight.
		path https://url/sessionsbyhighlights

	- getSessionsByDuration:
		Return requested conference sessions by duration.
		path https://url/sessionsbyduration

- Answer: The datastore support only one inequality per query, this query will return "BadRequestError", A way to bypass this limitation is to use query and merge the intersection of those too query.


