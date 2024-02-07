
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



