## i.Check The "Add to cart" flow again and Setup Cloud Functions Test.
##### ->Now Return to shopping app browser,Add an item to cart.
##### ->This is an important step to confirm that our tests and rules match the functionality.
##### ->So,till now we just focussed on the webapp and firesore security rules but it also has cloud functions which keeps the data added up to date.
##### ->But we know emulator suite will doesn't make it happen to test the cloud functions.so,to overcome that problem we should remove function.So,Open emulators-codelab/codelab-initial-state/functions/test.js and remove the function as in the below code.
            //  REMOVE .skip FROM THIS LINE
            describe.skip("adding an item to the cart recalculates the cart total. ", () => {

            it("should sum the cost of their items", async () => {
            });
            }); 

## j.Walk Through The Tests and Run The Tests
##### ->This test validates the interaction between Cloud Firestore and Cloud Functions, it involves more setup than the tests in the previous codelabs.
##### ->For this test, add two items that together cost 9.98. Then, check if the cart has the expected itemCount and totalPrice.IF it happened our webapp working perfectly.
            it("should sum the cost of their items", (done) => {
                const db = firebase
                    .initializeAdminApp({ projectId: REAL_FIREBASE_PROJECT_ID })
                    .firestore();

                // Setup: Initialize cart
                const aliceCartRef = db.doc("carts/alice")
                aliceCartRef.set({ ownerUID: "alice", totalPrice: 0 });

                //  Trigger calculateCart by adding items to the cart
                const aliceItemsRef = aliceCartRef.collection("items");
                aliceItemsRef.doc("doc1").set({name: "nectarine", price: 2.99});
                aliceItemsRef.doc("doc2").set({ name: "grapefruit", price: 6.99 });
                
                // Listen for every update to the cart. Every time an item is added to
                // the cart's subcollection of items, the function updates `totalPrice`
                // and `itemCount` attributes on the cart.
                // Returns a function that can be called to unsubscribe the listener.
                await new Promise((resolve) => {
                const unsubscribe = aliceCartRef.onSnapshot(snap => {
                    // If the function worked, these will be cart's final attributes.
                    const expectedCount = 2;
                    const expectedTotal = 9.98;
            
                    // When the `itemCount`and `totalPrice` match the expectations for the
                    // two items added, the promise resolves, and the test passes.
                    if (snap.data().itemCount === expectedCount && snap.data().totalPrice == expectedTotal) {
                    // Call the function returned by `onSnapshot` to unsubscribe from updates
                    unsubscribe();
                    resolve();
                    };
                });
                });
            });
##### -> Now run the test again by giving npm test in new terminal tab,And check whether emulators running,if not run emulators again.
##### ->After testing we can see still one function is failing,That means we are very close to the ending part.
##### -> after fixing the function webapp browser will be in running condition.

## k.Write a Function and Rerun the Tests
##### ->To debug this bug we need to update some lines in the function i.e., in functions/index.js
try {
        let totalPrice = 125.98;
        let itemCount = 8;

        const cartRef = db.collection("carts").doc(context.params.cartId);
        // ADD LINES FROM HERE
        const itemsSnap = await cartRef.collection("items").get();

        itemsSnap.docs.forEach(item => {
          const itemData = item.data();
        })
        // TO HERE
       
        return cartRef.update({
          totalPrice,
          itemCount
        });
      } catch(err) {
      }
##### ->Now save it and again run the test using npm test.Now we we can see all the tests are passing without any error.

## l.Try Using Store Front UI.
##### ->This is final step till now we just run the tests to check everying is running perfectly or not.
##### ->Now we have everything running perfectly,Now return to the web app and add few items in the cart.In the top-right we can see icon look alike as cart,click it we can see the no items and and total price of the items clearly.
