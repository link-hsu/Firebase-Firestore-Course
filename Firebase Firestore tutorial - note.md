## Firebase
[Course Link](https://www.youtube.com/playlist?list=PL4cUxeGkcC9itfjle0ji1xOZ2cjRGY_WB)
### Firestore Database
- Traits:
  - no-sql database
  - collection -> documents - like javascript object
- 取得表單資料三種範例:
  1. 
  ~~~
  form.addEventListener('submit', (e) => {
    e.preventDefault();
    db.collection('cafes').add({
        name: form.name.value,
        city: form.city.value
    });
    form.name.value = '';
    form.city.value = '';
  });
  ~~~
  2. document.querySelector
  ~~~
  const inputData = document.querySelector("form").elements;

  // 取得特定輸入資料
  const name = document.querySelector("form").elements["name"].value;
  ~~~
  3. FormData
  ~~~
  const formData = new FormData();

  // 將資料新增至 FormData 物件
  formData.append("name", "John Doe");
  formData.append("email", "johndoe@example.com");

  // 取得 FormData 物件中的資料
  const name = formData.get("name");
  const email = formData.get("email");

  // 將 FormData 物件傳送至伺服器端
  const xhr = new XMLHttpRequest();
  xhr.open("POST", "/api/submit");
  xhr.send(formData);
  ~~~
### Firebase 9
- version 9 v.s. version 8
  - v.9: use modular tree shaking: only import functions we need -> output smaller file size
  - v.8: object oriented approach
- setting up Webpack - [簡介](https://neighborhood999.github.io/webpack-tutorial-gitbook/Part1/ "Link here")
  - ```npm init -y```  
    ```npm i webpack webpack-cli -D```
  - [path.join() v.s. path.resolve()](https://hackernoon.com/whats-the-difference-between-pathjoin-and-pathresolve "Link here") in node.js
  - 1. create firebase project
    2. copy firebaseConfig object to index.js
      ~~~
      // import firebase from 'firebase/app';
      import { initializeApp } from 'firebase/app';

      // For Firebase JS SDK v7.20.0 and later, measurementId is optional
      const firebaseConfig = {
        apiKey: "AIzaSyCstmwclH-bWRJxuD0CKbMeaexC-Kmh0G8",
        authDomain: "eatnoodless-test.firebaseapp.com",
        projectId: "eatnoodless-test",
        storageBucket: "eatnoodless-test.appspot.com",
        messagingSenderId: "935536737043",
        appId: "1:935536737043:web:6d83807c859a90ff8b78e6",
        measurementId: "G-NXJLRRD059"
      };

      initializeApp(firebaseConfig);
      ~~~
    3. ```npm install firebase```

- [emmet - cheat sheet](https://docs.emmet.io/cheat-sheet/)
- firebase Auth
  - use **JSON** web token to authenticate
  - START -> E-mail and Password and Enabled
  - *Sign up*
    ~~~ javascript
    import { getAuth,
             createUserWithEmailAndPassword 
    } from "firebase/auth";
    const auth = getAuth();
    createUserWithEmailAndPassword(auth, email, password)
      .then((cred) => {
        console.log('user created', cred.user);
        signtupForm.reset();
      })
      .catch((err) => {
        console.log(err.message);
      });
    ~~~  
  - *LogOut*
    ~~~ javascript
    import { signOut } from "firebase/auth";
    signOut(auth)
      .then(() => {
        console.log('the user signed out');
      })
      .catch((err) => {
        console.log(err.message);
      });
    ~~~
  - *LogIn*
    ~~~ javascript
    import { signInWithEmailAndPassword } from "firebase/auth";
    signInWithEmailAndPassword(auth, email, password)
      .then((cred) => {
        console.log('user logged in: ', cred.user);
      })
      .catch((err) => {
        console.log(err.message);
      });
    ~~~
  - *Subscribing to Auth Changes*
    ~~~ javascript
    import { onAuthStateChanged } from "firebase/auth";
    onAuthStateChanged(auth, (user) => {
      console.log('user status changed: ', user);
    })
    ~~~
- *Unsubscribing from Changes*
  ~~~ javascript
  // return unSubscribe function
  const unsubCol = onSnapshot(q, (snapshot) => {
  let books = [];
  snapshot.docs.forEach(doc => {
    books.push({ ...doc.data(), id: doc.id })
  });
  console.log(books);
  });
  unsubCol();
  ~~~
