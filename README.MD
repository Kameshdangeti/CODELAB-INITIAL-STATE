
**Table Of Contents**

**Objective**: The guide below shows the steps for running,debugging, a simple online shopping app in codelabs.

## a.Prerequesites
#### ->A simple editor such as Visual Studio Code, Atom, or Sublime Text.
#### ->Node.js 10.0.0 or higher (to install Node.js, use nvm, to check your version, run node --version)
#### ->Java 7 or higher (to install Java use these instructions, to check your version, run java -version)

## b.Setup Code
#### ->In this codelab, you start off with a version of The Fire Store sample that is nearly complete, so the first thing you need to do is clone the source code.
##### -------------->git clone https://github.com/firebase/emulators-codelab.git
#### ->After cloning,change the directory of the codelab like this.
##### -------------->cd emulators-codelab/codelab-initial-state
#### ->Now Open the the folder in Visual Studio Code/Atom or Sublime Text.
#### ->Next,Install the dependencies so,we can run code.
#### ->change the directory to function
##### -------------->cd Functions
#### ->Install the dependencies 
##### -------------->npm install
#### ->move back to previuos directory
##### -------------->cd ../
#### ->Upto now we install the source code,But it is'nt sufficient we need to install firestore CLI too
##### -------------->npm install -g firebase-tools
#### ->Always check whether we are using updated version only.(Version should be 9.0.0 or more than that)
##### -------------->firebase --version
#### ->Now we got all the code for running the firebase emulator,Now we should connect the firebase project,
####   by creating it in the Firebase Console.
#### ->Now we should connect this code to firebase project.
##### -------------->firebase login
#### ->Next run the following command to create a project alias.
##### -------------->firebase use $YOUR_PROJECT_ID
## c.Run The Emulators
#### ->In this section we will run the emulators sing the follwing command.
##### -------------->firebase emulators:start --import=./seed 
#### ->Now emulators starts and output is occured.
#### ->Now we should connect our app to emulators by adding some code to public/js/homepage.js.
#### ->open the above file and add below code and save it.
##### -------------->const auth = firebaseApp.auth();
        const db = firebaseApp.firestore();


        // ADD THESE LINES
        if (location.hostname === "127.0.0.1") 
        {
            console.log("127.0.0.1 detected!");
            auth.useEmulator("http://127.0.0.1:9099");
            db.useEmulator("127.0.0.1", 8080);
        }git 
#### ->Now open the Emulator Ui using http://127.0.0.1:4000/.check the Authentication,firestore,functions,Hosting   Emulators are in ON mode.
#### ->Check all the items are imported in Firestore blog.



