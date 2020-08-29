# promise-timeout
An effective way to cancel gracefully a promise after a predefined timeout 

```
const PromiseTimeout = (ms: number, promise: Promise < any > ): Promise < any > => {
	// Create a promise that rejects in <ms> milliseconds
	const timeout = new Promise((resolve, reject) => {
		const id = setTimeout(() => {
			clearTimeout(id);
			reject('Timed out in ' + ms + 'ms.');
		}, ms)
	});

	// Returns a race between our timeout and the passed in promise
	return Promise.race([
		promise,
		timeout
	]);
}

export default PromiseTimeout;


// usage

const runRequest = PromiseTimeout(10000, <your promise object>);

try {
	const requestResponse = await runRequest;
	handleRequestResponse(requestResponse);
} catch (error) {
	console.log(`error: ${error}`);
	....
}
```
