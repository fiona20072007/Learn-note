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
