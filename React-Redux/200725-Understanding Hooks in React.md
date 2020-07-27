# 20200725 Understanding Hooks in React

![my-img](img/200725-1.png)

The translate itself is going to provide some options to the dropdown. It's also going to provide the currently selected item and a callback to change the currently selected item.

![my-img](img/200725-2.png)

Convert is going to accept two different props. First off, it's a prop called text. This is going to be the text that we want to convert into some other language. We're also going to take a prop called Language, and that is going to be one of the options out of our dropdown. So it will be an object with a label and a value property. We're going to provide those two props into the convert component.

we are going to setup the useEffect hook inside of convert. We're going to make sure that is configured so that anytime we provide a new language or text prop, we run that useEffect hook. Now, the Google Translate API itself is going to take the text and the target language code. It's going to do the translation internally and then send us back a response. Inside that response will be our translated text. We're going to take that translated text, use it to update some piece of state. And we're going to show that data on the screen.

![my-img](img/200725-3.png)

We have to make a post request to this exact you URL that you see right there and in the query parameters, we are going to provide a couple of different properties. First, we'll provide a q. q was going to be the actual text that we want to translate. We'll provide a target, which is going to be a language code. That is what we want to translate our text into. Besides Q and Target, we are also going to provide the key, this key parameter is going to be the valid API key. The response we get back is going to have a lot of information about the translated stuff.

```js
const Convert = () => {
  useEffect(() => {
    console.log("new language or text");
  }, [language, text]);
  return <div />;
};

const options = [
  {
    label: "Afrikaans",
    value: "af"
  },
  {
    label: "Arabic",
    value: "ar"
  },
  {
    label: "Hindi",
    value: "hi"
  }
];

const Translate = () => {
  const [language, setLanguage] = useState(options[0]);
  const [text, setText] = useState("");

  return (
    <div>
      <div className="ui form">
        <div className="field">
          <label>Enter Text</label>
          <input value={text} onChange={e => setText(e.target.value)} />
        </div>
      </div>
      <Dropdown
        label="Select a Language"
        selected={language}
        onSelectedChange={setLanguage}
        options={options}
      />
      <hr />
      <h3 className="ui header">Output</h3>
      <Convert text={text} language={language} />
    </div>
  );
};
```

we know that we need to make a request whenever the value of language or text changes in any way. The ideal way of wiring that up is to put together a use effect hook.

We can now start to make our request to the Google API from inside this use effect function. The first thing we need to do is make sure that we install Axios into our project.

```js
npm install axios
```

```js
const Convert = ({ language, text }) => {
  useEffect(() => {
    axios.post(
      "https://translation.googleapis.com/language/translate/v2",
      {},
      {
        params: {
          q: text,
          target: language.value,
          key: "AIzaSyCHUCmpR7cT_yDFHC98CZJy2LTms-IwDlM"
        }
      }
    );
  }, [language, text]);
  return <div />;
};
```

whenever we make a post request, the second argument to Axios is always gonna be some information to send along in the body. We should provide these as query string parameters, even though we're making a post request, we're not gonna send anything along in the body. Instead, we're going to send all of our data along as query string parameters. And to do so, we have to provide a third argument. A third object to the Axios.Post statement.

We're going to leave the second argument as an empty object to say that we don't want to send along any information in the body, the request. Instead, we want to provide a third argument (second object). Inside there is going to be a params property. And that's where we are going to add all of our query string parameters.

whenever our convert component is first displayed on the screen, the use effect function is going to run. And so we can see already that there has been one request made to that translation API. I've got a status of 200 right here.
