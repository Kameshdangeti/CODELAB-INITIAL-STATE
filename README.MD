
**Table Of Contents**

**Objective**: The guide below shows the steps for running,debugging, a simple online shopping app in codelabs.

## a.Prerequesites
##### ->A simple editor such as Visual Studio Code, Atom, or Sublime Text.
##### ->Node.js 10.0.0 or higher (to install Node.js, use nvm, to check your version, run node --version)
##### ->Java 7 or higher (to install Java use these instructions, to check your version, run java -version)

## b.Setup Code
##### ->In this codelab, you start off with a version of The Fire Store sample that is nearly complete, so the first thing you need to do is clone the source code.
###### -------------->git clone https://github.com/firebase/emulators-codelab.git
##### ->After cloning,change the directory of the codelab like this.
###### -------------->cd emulators-codelab/codelab-initial-state
##### ->Now Open the the folder in Visual Studio Code/Atom or Sublime Text.
##### ->Next,Install the dependencies so,we can run code.
##### ->change the directory to function
###### -------------->cd Functions
##### ->Install the dependencies 
###### -------------->npm install
##### ->move back to previuos directory
###### -------------->cd ../
##### ->Upto now we install the source code,But it is'nt sufficient we need to install firestore CLI too
###### -------------->npm install -g firebase-tools
##### ->Always check whether we are using updated version only.(Version should be 9.0.0 or more than that)
###### -------------->firebase --version
##### ->Now we got all the code for running the firebase emulator,Now we should connect the firebase project,
#####   by creating it in the Firebase Console.
##### ->Now we should connect this code to firebase project.
###### -------------->firebase login
##### ->Next run the following command to create a project alias.
###### -------------->firebase use $YOUR_PROJECT_ID
## c.Run The Emulators
##### ->In this section we will run the emulators sing the follwing command.
###### -------------->firebase emulators:start --import=./seed 
##### ->Now emulators starts and output is occured.
##### ->Now we should connect our app to emulators by adding some code to public/js/homepage.js.
##### ->open the above file and add below code and save it.
###### -------------->const auth = firebaseApp.auth();
        const db = firebaseApp.firestore();


        // ADD THESE LINES
        if (location.hostname === "127.0.0.1") 
        {
            console.log("127.0.0.1 detected!");
            auth.useEmulator("http://127.0.0.1:9099");
            db.useEmulator("127.0.0.1", 8080);
        }git 
##### ->Now open the Emulator Ui using http://127.0.0.1:4000/.check the Authentication,firestore,functions,Hosting   Emulators are in ON mode.
##### ->Check all the items are imported in Firestore blog.

## d.Run and Debugg the App
##### ->Firstly open the app by using the http://127.0.0.1:5000 you can see the webiste of shopping app.
##### ->In that pick an item add to cart but we get error.So we need to fix that bug by debugging it.
##### ->To find the bug we need click ctrl+shift+j (windows,linux,chrome) and cmd+opt+j (Mac) in browser.
##### ->We can see that error will be in the add to cart, so we should fix that code in public.js/homepage.js by adding some code init as:
        addToCart(id, itemData) {
        // ADD THESE LINES
        if (this.auth.currentUser === null) {
        this.showError("You must be signed in!");
        return;
        }
    } 
##### ->Now refersh the page and add items to cart we should see the error as you must be signe in.

## e.Create Security rules inbuilted for app.
##### ->In the editor, open the file emulators-codelab/codelab-initial-state/firestore.rules. You'll see that we have three main sections in our rules.
##### ->Right now anyone can read and write data to our database! We want to make sure that only valid operations get through and that we don't leak any sensitive information.
##### ->Let's update the first two rules to deny access by setting the condition to false:
        rules_version = '2';
        service cloud.firestore {
        match /databases/{database}/documents {
            // User's cart metadata
            match /carts/{cartID} {
            // UPDATE THIS LINE
            allow read, write: if false;
            }

            // Items inside the user's cart
            match /carts/{cartID}/items/{itemID} {
            // UPDATE THIS LINE
            allow read, write: if false;
            }

            // All items available in the store. Users can read
            // items but never write them.
            match /items/{itemID} {
            allow read: if true;
            }
          }
        }
## f.Run the emulators and test it
##### ->Now start the emulators again,if it is noy running
##### ->Now open new command line and move to functions directory and run the tests:
###### --------------->npm test
##### ->We can see 4 failing.

## g.Secure Cart Access
##### ->The first two failures are the "shopping cart" tests which test that:
###### Users can only create and update their own carts.
###### Users can only read their own carts.
##### ->open the security rules file, firestore.rules, and update the statements within match /carts/{cartID}
        rules_version = '2';
        service cloud.firestore {
            // UPDATE THESE LINES
            match /carts/{cartID} {
            allow create: if request.auth.uid == request.resource.data.ownerUID;
            allow read, update, delete: if request.auth.uid == resource.data.ownerUID;
            }
        }
##### ->Now test again now we can see only 2 failing.

## h.Allow Cart Item acces And Test
##### ->The remaining 2 failures can be overcome by updating these lines in firestore.rules.
        rules_version = '2';
        service cloud.firestore {
        match /databases/{database}/documents {
            // ...

            // UPDATE THESE LINES
            match /carts/{cartID}/items/{itemID} {
            allow read, write: if get(/databases/$(database)/documents/carts/$(cartID)).data.ownerUID == request.auth.uid;
            }

            // ...
        }
        }
##### ->Now test again we get 0 failures but one pending item,we should look into it.





