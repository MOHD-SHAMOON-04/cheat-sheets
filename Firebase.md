# Firebase Web SDK Cheat Sheet

## Getting Started

1. **Install Firebase SDK**
    ```html
    <!-- Add Firebase SDKs -->
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-auth.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-storage.js"></script>
    ```

2. **Initialize Firebase**
    ```javascript
    // Your web app's Firebase configuration
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_AUTH_DOMAIN",
      projectId: "YOUR_PROJECT_ID",
      storageBucket: "YOUR_STORAGE_BUCKET",
      messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
      appId: "YOUR_APP_ID"
    };

    // Initialize Firebase
    const app = firebase.initializeApp(firebaseConfig);
    ```

## Authentication

1. **Sign Up with Email and Password**
    ```javascript
    const auth = firebase.auth();
    auth.createUserWithEmailAndPassword(email, password)
      .then((userCredential) => {
         // Signed up
         const user = userCredential.user;
      })
      .catch((error) => {
         console.error(error);
      });
    ```

2. **Sign In with Email and Password**
    ```javascript
    auth.signInWithEmailAndPassword(email, password)
      .then((userCredential) => {
         // Signed in
         const user = userCredential.user;
      })
      .catch((error) => {
         console.error(error);
      });
    ```

3. **Sign Out**
    ```javascript
    auth.signOut().then(() => {
      // Sign-out successful
    }).catch((error) => {
      console.error(error);
    });
    ```

## Firestore Database

1. **Get a Firestore Instance**
    ```javascript
    const db = firebase.firestore();
    ```

2. **Add Data**
    ```javascript
    db.collection("users").add({
      first: "Ada",
      last: "Lovelace",
      born: 1815
    })
    .then((docRef) => {
      console.log("Document written with ID: ", docRef.id);
    })
    .catch((error) => {
      console.error("Error adding document: ", error);
    });
    ```

3. **Read Data**
    ```javascript
    db.collection("users").get().then((querySnapshot) => {
      querySnapshot.forEach((doc) => {
         console.log(`${doc.id} => ${doc.data()}`);
      });
    });
    ```

4. **Update Data**
    ```javascript
    const userRef = db.collection("users").doc("USER_ID");
    userRef.update({
      last: "Lovelace"
    })
    .then(() => {
      console.log("Document successfully updated!");
    })
    .catch((error) => {
      console.error("Error updating document: ", error);
    });
    ```

5. **Delete Data**
    ```javascript
    db.collection("users").doc("USER_ID").delete().then(() => {
      console.log("Document successfully deleted!");
    }).catch((error) => {
      console.error("Error removing document: ", error);
    });
    ```

## Storage

1. **Get a Storage Reference**
    ```javascript
    const storage = firebase.storage();
    const storageRef = storage.ref();
    ```

2. **Upload File**
    ```javascript
    const file = ...; // use the File API to get a file
    const uploadTask = storageRef.child('images/' + file.name).put(file);

    uploadTask.on('state_changed', 
      (snapshot) => {
         // Observe state change events such as progress, pause, and resume
      }, 
      (error) => {
         console.error(error);
      }, 
      () => {
         // Handle successful uploads on complete
         uploadTask.snapshot.ref.getDownloadURL().then((downloadURL) => {
            console.log('File available at', downloadURL);
         });
      }
    );
    ```

3. **Download File**
    ```javascript
    storageRef.child('images/' + fileName).getDownloadURL().then((url) => {
      // `url` is the download URL for 'images/fileName'
      console.log('File available at', url);
    }).catch((error) => {
      console.error(error);
    });
    ```

## Hosting

1. **Install Firebase CLI**
    ```bash
    npm install -g firebase-tools
    ```

2. **Login to Firebase**
    ```bash
    firebase login
    ```

3. **Initialize Project**
    ```bash
    firebase init
    ```

4. **Deploy to Firebase Hosting**
    ```bash
    firebase deploy
    ```
[Firebase documentation](https://firebase.google.com/docs).