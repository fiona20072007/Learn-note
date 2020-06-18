# 20200618 React-Redux

We are choosing to import the action creator and then pass it off to our connect function as opposed to just directly calling the action creator inside of our component.

![my-img](img/200618-1.png)

```js
// Action creator
export const selectSong = song => {
  // Return an action
  return {
    type: "SONG_SELECTED",
    payload: song
  };
};
```

Our actions index.js file contains our action creator. Now we are referring to this function as an action creator, but there's nothing about this function right here that somehow automatically wires itself into redux. There's nothing inside this function that somehow registers the function with redux.

There's nothing to say that anytime that I call this function and return that action that redux is going to somehow magically detect that and take the action and threw it off to all those reducers and update our state. That does not happen at any step.

So when we are inside of our SongList component, and we import the selectSong action creator, we're not so much importing action creator as we are importing a regular plain old javascript function.

So we can call selectSong inside of SongList component as many times as you want, but this is going to be treated as a normal javascript function call. We're going to call the function, it's going to return an object, the object that gets returned from this function call never gets forwarded over to redux, redux doesn't detect that we called a function that you and I are arbitrarily calling a action creator.

If we ever want to make sure that an action eventually makes its way over to our reducers, we have to call that dispatch function, we have to take the action that gets returned and we have to pass it into the dispatch function.

In CodePen example, notice how every single time that we dispatched an action, we called our action creator that returned an action object. And then that was directly passed into the store.dispatch function

If we had not passed the result of calling those action creators, if we had instead simply called createPolicy('Alex', 20), none of these function calls are going to update our Redux store. They return actions, but those actions never got sent into redux and they never ended up inside of our reducers. (console.log(store.getState()); you'll notice that I have no policy)

So if we ever want to make sure that an action creator actually updates our states, we have to take the action that gets returned and send it into that dispatch function.

```js
export default connect(
  mapStateToProps,
  { selectSong }
)(SongList);
```

Now you do not actually see any reference to dispatch inside of our component. When we pass our action creators into the connect function, the connect function does a special operation on the functions inside of this object. It looks at all the functions include inside of this object, and it essentially wraps them up in another javascript function.

When we call the new javascript function, the connect function is going to automatically call our action creator, it's going to automatically take the action that gets returned and it's going to automatically call the dispatch function for us.

So by passing our action creator into that connect function whenever we call the props.action creator or the action creator that gets added to our prop's object, the connect function is going to automatically take the action that gets returned and throw it into the dispatch function for us.

So any time that you ever want to call a action creator from a component you are always going to pass it into this connect function.

---

![my-img](img/200618-2.png)

the SongDetail component needs to know information that is stored inside of our redux store, so we wrap SongDetail with the connect component.

We define a mapStateToProps function that's going to tell the provider to tell it anytime the selected song changes. In response, the provider tell the connect function every single time that the user clicks on a select button and the selected song changes.

The connect function take the currently selected song and pass it as a prop down into the SongDetail component.

Now the SongDetail component has absolutely no functionality (allow user to click on something... which would cause a change to a redux state) tied to it, so we do not need to wire up any action creator to SongDetail component.

(You do not need to make a class based component to work with the connect function, you can work with the connect function with a functional component as well.)
