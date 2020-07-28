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

```js
const Convert = ({ language, text }) => {
  const [translated, setTranslated] = useState("");

  useEffect(() => {
    const doTranslation = async () => {
      const { data } = await axios.post(
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
      setTranslated(data.data.translations[0].translatedText);
    };

    doTranslation();
  }, [language, text]);
  return <div />;
};
```

So now inside of our use effect function, we need to make sure that we get access to the response from that request. Remember that whenever we make a request inside of use effect, we cannot directly use the async await syntax. Instead, we have to either wrap this request with another function or we can fall back to using the promised syntax instead. In this case, let's just make a little helper function and market as async.

We'll then call setTranslated and we'll pull our information or the translated text out of that data object. Remember that when we get the response back from Axios, all the information inside that response is available on the data property. We just destructure that out. But further inside that response, inside of the information that we actually get back from the Google API, there is also a data property. So there are two data properties. The first one is the information inside of the Axios response. And inside of that response, there is also a data property. The second one is the actual response data. We want to take the first translation result out of there and get the translated text property. Make sure that after defining this entire do translation function, we actually call it inside of use effect. That function is going to be invoked anytime we first mount our component, any time we change language and text.

---

![my-img](img/200725-4.png)

we're going to add in a second piece of state called deBouncedText. We are then going to create two separate use effect functions. The first use effect function is going to run whenever the text prop changes. The second use effect function is only gonna run whenever the deBouncedText piece of state changes.

whenever user types into that text input, we are going to set up a timer to update the deBouncedText piece of state in five milliseconds. If the user types into that input again, and causes a new value for the text prop to flow into our component, we're then going to execute a cleanup function that is going to cancel the timer we had previously set. So in other words, as the user types into that input, we're going to set up a timer and then cancel it, set up a timer, cancel it, until the user types, and then pauses for five hundred milliseconds.

As soon as they pause for that five milliseconds, we will go ahead and execute the update to the deBouncedText piece of state. And that will cause our use effect number two to execute. inside of use effect number two, we're going to make a request over to the Google API with the deBouncedText piece of state as the thing that we want to translate.

```js
useEffect(() => {
  const timerId = setTimeout(() => {
    setDebouncedText(text);
  }, 500);

  return () => {
    clearTimeout(timerId);
  };
}, [text]);
```

We want to rerun this hook whenever the text prop changes. Next up, we will define our timer. And I want to run this arrow function right here after five hundred milliseconds. Then inside that timer, as soon as this function right here actually executes. So with the 500 milliseconds goes by, we don't cancel this timer. We want to update our deBouncedText piece of state to whatever text currently is. If the text piece of state changes before that 500 milliseconds elapses, we want to cancel that timer. So we'll make sure that we return a cleanup function.

```js
useEffect(() => {
  const doTranslation = async () => {
    const { data } = await axios.post(
      "https://translation.googleapis.com/language/translate/v2",
      {},
      {
        params: {
          q: debouncedText,
          target: language.value,
          key: "AIzaSyCHUCmpR7cT_yDFHC98CZJy2LTms-IwDlM"
        }
      }
    );
    setTranslated(data.data.translations[0].translatedText);
  };

  doTranslation();
}, [language, debouncedText]);
```

use effect number two, all we want to do is make the request using the deBouncedText piece of state. And then finally, we're going to make sure that we only run this function when deBouncedText changes.

Do we have to do all the same debouncing stuff for language? Well, probably not, because a user is not going to be changing that language so quickly that we'd want to limit the number of requests we make. It's really just the typing action that we would want to somehow limit.
