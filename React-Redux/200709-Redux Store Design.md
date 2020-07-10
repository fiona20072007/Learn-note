# 20200709 Redux Store Design

```js
export default (state = [], action) => {
  switch (action.type) {
    case "FETCH_USER":
      return [...state, action.payload];
    default:
      return state;
  }
};
```

we wire up a reducer to catch that action that is now being dispatched from our actual creator. And this new usersReducer is going to maintain a array or a simple list of all the users that we have fetched inside of our application. Once we collect that data inside the reducer, we can then make it available to our UserHeader component, and then the UserHeader component can pick out the user that it cares about and shows some details about it on the screen.

```js
//inside reducers index.js file
export default combineReducers({
  posts: postsReducer,
  users: usersReducer
});
```

So we'll now take the reducer and make sure that we wire it up to our combined reducers call

```js
const mapStateToProps = state => {
  return { users: state.users };
};
```

this component needs to get access to our redux level state, anytime we want to do that we will define the mapStateToProps function.

```js
render() {
    const user = this.props.users.find(user => user.id === this.props.userId);
    //this will find just the user that we care about.
    return <div>User Header</div>;
}
```

So now our component is going to have access to a prop called this.props.users that's going to be the array of all the users that we care about.

Inside the render method we're going to look through that array and find the user that we want to display.

find is a built in method on Javascript arrays, we can call it with a function. This function is going to be invoked with every element inside of that array. And then as soon as we return true from this inner function right here, the entire find function is going to stop and return whatever element we had return true for.

when the application first is rendered onto the screen we will not have a list of users here, we will have an empty array but the user that we care about is not going to actually be available.

```js
render() {
    const user = this.props.users.find(user => user.id === this.props.userId);
    if (!user) {
      return null;
    }
    return <div>{user.name}</div>;
}
```

If we did not find some appropriate user then we will just return nothing for right now. If we did find a user, we will instead put the user's name inside of here.

---

inside of our UserHeader component, we are passing this thing a collection of different props. Right now this thing gets access to the userId, and it also gets access to the entire list of users.

The UserHeader component is not really well-suited to take the entire array of users. essentially the entire purpose of the UserHeader component is to show one singular user on the screen. It only has to show one user but we're giving it way more data than it ever needs.(we are giving it the entire list of users)

And so to make this component by itself a little bit more reusable, it would be a lot better if we could figure out some way to pass it just the user that it cares about. To do so, there's a couple of different approaches we could take.

---

one approach

```js
class PostList extends React.Component {
  componentDidMount() {
    this.props.fetchPosts();
  }

  renderList() {
    return this.props.posts.map(post => {
      return (
        <div className="item" key={post.id}>
          <i className="large middle aligned icon user" />
          <div className="content">
            <div className="description">
              <h2>{post.title}</h2>
              <p>{post.body}</p>
            </div>
            <UserHeader userId={post.userId} />
          </div>
        </div>
      );
    });
  }
```

We could either get our entire list of users inside of PostList, and then every single time that we render the UserHeader, we could go through that find logic and attempt to just pass in the user that we care about as opposed to this userId={post.userId}.

---

another approach

inside of UserHeader component, I'm going to go down to the mapStateToProps function. The reason that we defined mapStateToProps right here is so that we can do kind of like these pre calculations on values that are coming into our component as props and our redux state.

So rather than passing in like a ton of data to our component and relying upon the component to figure out how to find the user that we care about, we could instead extract all that logic to the mapStateToProps function. So rather than going through this find operation inside the components itself, write that logic out inside of mapStateToProps.

In some applications like some professional applications some engineers decide to define this mapStateToProps function and this connect stuff inside of a separate file.

So you might have one file that has mapStateToProps and the initial connect set up right here, and then in a different file you will define the component by itself. The advantage to that is that you will have a component that can be used on its own without having to reach into the redux store.

But then if you want to you can also very easily create a version of that component that will reach into the redux store and get all of its own data. In addition if you ever have another component that needs to get access to just one particular user given only a user ID we could easily write mapStateToProps and this connect stuff into a separate file and then reuse it in multiple components inside of our application.

```js
const mapStateToProps = (state, ownProps) => {
  return { user: state.users.find(user => user.id === ownProps.userId) };
};
```

mapStateToProps does not only get called with our state object out of our redux store as an argument, it actually gets a second argument as well, that is referred to as ownProps. This ownProps object is a reference to the props that are about to be sent into this component.

So if we ever want to do pre calculation steps, so that we don't have to pass in a ton of data directly to the components, we can reference the props that are about to go into the components on this ownProps object, and then we still get our redux state on the first argument.

```js
render() {
    const { user } = this.props;
    if (!user) {
      return null;
    }
    return <div className="header">{user.name}</div>;
}
```

inside my render method we no longer have access to the this.props.users value, instead it's simply this.props.user because now we are only passing in the one user to our components.

I'm going to destructure off just that user from my props object. And so now we can check to see if that user exists.

---

the entire idea here is that we can extract anything that is going to do some computation on our state our redux States and the prop's coming into our component to the mapStateToProps function. And that's really what mapStateToProps is for.
