## Keyboard events & Keyboard properties

### Code

```const function_name = (e) => {
    // execute code
    console.log(e.key)
};
itemInput.addEventListener(${property}, function_name)
```

### Properties

- `keypress`: tap in key or hold it, it will fire off once
- `keydown`: tap and it will fire until key is pressed
- `keyup`: tap and it will fire off once you release

## Input Events

### Code

```const itemInput = document.ElementById('item-input')

const function_name = (e) => {
    // execute code
    console.log(e.target.value)
};
itemInput.addEventListener(${property}, function_name)
```

### Properties

- `input`: take the input value
- `change`: take the value, used when selected
- `blur`: used to make the element unfocussed when not in use
- `focus`: used to make the element focused when is use

## Form Submission & FormData Object
### Code

```const form = document.ElementById('item-form')

const onSubmit = (e) => {
    // option 1
    const item = document.getElementById('item-input').value;

    // option 2
    const formData = new FormData(form);
    formData.get('item');
    //or
    const entries = formData.entries();
    for (let entry of entries) {
        console.log(entry[1]);
    }

};
itemInput.addEventListener(${property}, function_name)
```

### Properties

- `FormData`: best approach to get form entries

## Event Bubbling

### Code

`````const child = document.querySelector('child');
const parent1 = document.querySelector('parent1');
const parent2 = document.querySelector('parent2');
const parent3 = document.querySelector('parent3');

/*
If I click child, it will sequentially call all the parent elements one by one. Its same with the other elements and it will start calling parent elements. It can be quite tidious. To stop that, we can add stopPropogation event.
*/

child.addEventListener('click', (e)=>{
    alert('child was clicked');
    e.stopPropogation();
});

parent1.addEventListener('click', (e)=>{
    alert('Parent1 was clicked');
});

parent2.addEventListener('click', (e)=>{
    alert('Parent2 was clicked');
});

parent3.addEventListener('click', (e)=>{
    alert('Parent3 was clicked');
});````
`````

## Event Delegation & Multiple Events

Event Delegation: For multiple items, instead of adding event listeners to mutiple elements, we can get 'ul' and delegate event action to all the elements inside that 'ul'

```const listItems = document.querySelectorAll('li');
const list = document.querySelectorAll('ul');

listItme.forEach((item) => {
    item.addEventListener('click', (e) => {
        e.target.remove();
    });
});
list.addEventListener('click', (e) => {
    e.target.remove();
});
```

## MIssing sone

## Callbacks

When we want some function to execute after the execution of that function. We set it in callback

```const posts = [
    {
        title:'Title one';
        dscription: 'Description one';
    },
    {
        title:'Title two';
        dscription: 'Description two';
    }
]

function creatPost(post,cb) {
    setTimeout(()=>{
        posts.push(post);
        cb();
    },20000);
}

function getPosts () {
    setTimeout(()=>{
        posts.forEach(function (post) {
            console.log(`${post.title} - ${post.description}`);
        });
    }, 1000);
}

createPosts({title:'Title one', dscription: 'Description one'}, getPosts);
```

## AJAX(async js and xml) & XHR(XMLHttpRequest) Object

A built in browser object that allows us to make Http requests. We can make requests to servers without having to refresh the page. This allows us to make our webppages more dynamic. Nowadays you'll use fetch API (JSON) than XHR.

AJAX engine converts JS call into XHR when sending from client to server. And converts JSON or XML to HTML response when sending form server to client

```const xhr = new XMLHttpRequest();

xhr.open('GET', 'https://github.com/users/manmeetskalra/repos');

xhr.onreadystatechange() = function () {
    if(this.readyState===4 &&& this.status===200){
        const data = JSON.parse(this.responseText);
        data.forEach((repo) => {
            const li = document.createElement('li');
            li.innerHTML = `<strong>${repo.name}</strong> - ${repo.description}`;
            document.querySelector('#results).appendChild(li);
        });
    }
}
```

## Callback Hell

When you pass callbacks within callbacks, it become nested callbacks that is difficult to follow, this it is called callback hell

```function getData(endpoint, cb) {
  const xhr = new XMLHttpRequest();

  xhr.open('GET', endpoint);

  xhr.onreadystatechange = function () {
    if ((this.readyState === 4) & (this.status === 200)) {
      cb(JSON.parse(this.responseText));
    }
  };

  setTimeout(() => {
    xhr.send();
  }, Math.floor(Math.random() * 3000) + 1000);
}

getData('./movies.json', (data) => {
  console.log(data);
  getData('./actors.json', (data) => {
    console.log(data);
    getData('./directors.json', (data) => {
      console.log(data);
    });
  });
});
```

###Promises

Promise is an object that tells us eventual completion or failure of an asynchronous operation. Promise is the promise to the script telling us whether its success or failed

```
// Create a promise
const promise = new Promise((resolve, reject) => {
  // Do some async task
  setTimeout(() => {
    console.log('Async task complete');
    resolve();
  }, 1000);
});

// promise.then(() => {
//   console.log('Promise consumed..');
// });

const getUser = new Promise((resolve, reject) => {
  setTimeout(() => {
    let error = true;

    if (!error) {
      resolve({ name: 'John', age: 30 });
    } else {
      reject('Error: Something went wrong');
    }
  }, 1000);
});

getUser
  .then((user) => console.log(user))
  .catch((error) => console.log(error))
  .finally(() => console.log('The promise has been resolved or rejected'));

console.log('Hello from global scope');
```

## Callback To Promise Refactor

```const posts = [
  { title: 'Post One', body: 'This is post one' },
  { title: 'Post Two', body: 'This is post two' },
];

function createPost(post) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      let error = false;

      if (!error) {
        posts.push(post);
        resolve();
      } else {
        reject('Something went wrong');
      }
    }, 2000);
  });
}

function getPosts() {
  setTimeout(() => {
    posts.forEach(function (post) {
      const div = document.createElement('div');
      div.innerHTML = `<strong>${post.title}</strong> - ${post.body}`;
      document.querySelector('#posts').appendChild(div);
    });
  }, 1000);
}

function showError(error) {
  const h3 = document.createElement('h3');
  h3.innerHTML = `<strong>${error}</strong>`;
  document.getElementById('posts').appendChild(h3);
}

createPost({ title: 'Post Three', body: 'This is post' })
  .then(getPosts)
  .catch(showError);
```

## Promise Chaining

Used if you have sequence of async task to complete or if you have a promise that returns a value that is used in the other promise.

```const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    let error = true;

    if (!error) {
      resolve({ name: 'John', age: 30 });
    } else {
      reject('Error: Something went wrong');
    }
  }, 1000);
});

promise
  .then((user) => {
    console.log(user);
    return user.name;
  })
  .then((name) => {
    console.log(name);
    return name.length;
  })
  .then((nameLength) => {
    console.log(nameLength);
  })
  .catch((error) => {
    console.log(error);
    return 123;
  })
  .then((x) => console.log('This will run no matter what', x));
```

## Promises vs Callback Hell

```function getData(endpoint) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();

    xhr.open('GET', endpoint);

    xhr.onreadystatechange = function () {
      if (this.readyState === 4) {
        if (this.status === 200) {
          resolve(JSON.parse(this.responseText));
        } else {
          reject('Something went wrong');
        }
      }
    };

    setTimeout(() => {
      xhr.send();
    }, Math.floor(Math.random() * 3000) + 1000);
  });
}

// Whatever we return from a .then() is passed into the next .then() callback function args
getData('./movies.json')
  .then((movies) => {
    console.log(movies);
    return getData('./actors.json');
  })
  .then((actors) => {
    console.log(actors);
    return getData('./directors.json');
  })
  .then((directors) => {
    console.log(directors);
  })
  .catch((error) => console.log(error));
```

## Handling Multiple Promises with promise.all()

```function getData(endpoint) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();

    xhr.open('GET', endpoint);

    xhr.onreadystatechange = function () {
      if (this.readyState === 4) {
        if (this.status === 200) {
          resolve(JSON.parse(this.responseText));
        } else {
          reject('Something went wrong');
        }
      }
    };

    setTimeout(() => {
      xhr.send();
    }, Math.floor(Math.random() * 3000) + 1000);
  });
}

const moviesPromise = getData('./movies.json');
const actorsPromise = getData('./actors.json');
const directorsPromise = getData('./directors.json');

const dummyPromise = new Promise((resolve, reject) => {
  resolve('Hello World');
});

// Takes in promises
Promise.all([moviesPromise, actorsPromise, directorsPromise, dummyPromise])
  .then((data) => {
    // Returns an array of promise results
    console.log(data);
  })
  .catch((error) => console.log(error));
```

## Fetch Basics

Fetch API returns promises. New way to make Http requests

```// Fetching a JSON file
fetch('./movies.json')
  .then((response) => response.json())
  .then((data) => console.log(data));

// Fetching a text file
fetch('./test.txt')
  .then((response) => response.text())
  .then((data) => console.log(data));

// Fetching from an API
fetch('https://api.github.com/users/bradtraversy')
  .then((response) => response.json())
  .then((data) => (document.querySelector('h1').textContent = data.login));
```

## Fetch Options - Method, Body Headers

```/*
  COMMON FETCH OPTIONS
  method: HTTP method you want to use
  body: Data you want to send. Usually to be put in a database, etc
  headers: Any HTTP headers you want to send
*/

function createPost({ title, body }) {
  fetch('https://jsonplaceholder.typicode.com/posts', {
    method: 'POST',
    body: JSON.stringify({
      title,
      body,
    }),
    headers: {
      'Content-Type': 'application/json',
      token: 'abc123',
    },
  })
    .then((res) => res.json())
    .then((data) => console.log(data));
}

createPost({ title: 'My Post', body: 'This is my Post' });
```

## Name

Description

```Code

```
