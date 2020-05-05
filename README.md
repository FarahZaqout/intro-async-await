# Async/Await

Async/Await is an abstraction over promises. It allows you to write asynchronous code in a cleaner way than promsies without losing any features.

Let's get right into it.

## A Promise Example:

Let's create a very simple server. It has a `/users` endpoint and it fetches a bunch of fake data.

- `mkdir async-await-example`
- `cd async-await-example`
- `touch index.js`
- `npm i express axios`

Inside `index.js`:

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
	res.send('hello code academy');
});
app.get('/users', (req, res) => {
	console.log(req.url);
});

app.listen(3000, () => {
	console.log('it is working');
});
```

Now we have a working server. Let's make the `/users` endpoint work.

There's a useful website for testing API calls on fake data called [jsonplaceholder](https://jsonplaceholder.typicode.com/). We can use it to fetch random user data.

So let's make a call to this api and see what result it returns:

```javascript
app.get('/users', (req, res) => {
axios
	.get('https://jsonplaceholder.typicode.com/users')
	.then(console.log)
	.catch(console.log);
});
```

It should show me a large response object. We are only interested in the `data` key in it.

```javascript
app.get('/users', (req, res) => {
axios
	.get('https://jsonplaceholder.typicode.com/users')
	.then((result) => {
		const { data } = result;
		res.json(data);
	})
	.catch(console.log);
});
```

now restart the server and see the result in the browser.

---

## Using Async/Await:

Now that the example works, let's convert our promise syntax into async await.

1- Remove the `.then` and `.catch`. We won't be needing them.
2- add the keyword `async` to your controller callback.

```javascript
app.get('/users', async (req, res) => {
	axios.get('https://jsonplaceholder.typicode.com/users');
});
```

3- once we add the keyword `async` to a function, we can use the keyword `await` in front of a promise.

```javascript
app.get('/users', async (req, res) => {
	const result = await axios.get('https://jsonplaceholder.typicode.com/users');
	const { data } = result;
	res.json(data);
});
```

**The result of the `await` statement is what we get inside our `.then` callback.**

## Error Handling in Async/Await:

In order to handle errors in async/await we can use `.catch` after an `await` statement. However, when we are doing multiple awaits, there are better options for us.

We will use try/catch instead.

```javascript
app.get('/users', async (req, res) => {
	try {
		const result = await axios.get(
			'https://jsonplaceholder.typicode.com/users'
		);
		const { data } = result;
		res.json(data);
	} catch (e) {
		console.log(e);
	}
});
```

in the code above, we have 2 new blocks:

- `try`: This is our execution block. Inside it we will write all our code. If an error is thrown inside the `try` block, it well be sent to the `catch` block.
- `catch`: Does the same as catch in promsies. We write our error handling logic here.

**That's async/await in a nutshell.**
