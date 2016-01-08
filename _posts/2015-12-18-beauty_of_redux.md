---
title: The beauty of Redux
updated: 2015-12-18
---

# The beauty of Redux


-----
Edit 2015-01-08

As it turns out I was not thinking straight when I wrote this blogpost. The best approach for this problem was infact to use params and that was also super easy to do with React Router once I learned the correct syntax. So do not take this blogpost as the truth but rather look at it as a part of my progress in mastering React and Redux.

-----

One of the things I spent some time thinking about was how to access the data of a specific note when that note is selected. I have a list with all notes and when one note is selected, you get redirected to a page with detailed info about that note.
My initial (stupid) thought was to send the note id as a paramether in the route and then get that specific note from firebase. So I started to read up about how to send params in React-Router and while I understand that is possible i didn't get it to work. After some frustrating thinking and trying I realized I was aproaching the problem all wrong. Redux to the rescue!

I created a viewNoteDetails action where i send in the note as a paramether:

```javascript
viewNoteDetails: function(note){
        return function(dispatch, getState){
            dispatch({
                type: C.NOTE_DETAILS,
                data: {
                    user: note.user,
                    title: note.title,
                    content: note.content,
                    key: note.key
                }
            });
        }
    }
```

In the action I dispatch a type and the note data to a NotesReducer:

```javascript
const NotesReducer = function(state, action){
    switch(action.type){
        case C.NOTE_DETAILS:
            return Object.assign({}, state, {
               data: action.data
            });
            default:
            return state || initialState.notes;
    }
}
```

Then in my Note component were I want to display the note info I can access the note like this: 

```javascript
let note = this.props.notes.data 
```

Super smooth and no additional calls to firebase is necessary.