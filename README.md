# firestore-lift

## Purpose

Wrapper for firestore that provides types for the documents and most common firestore functions such as queries.

## Example

See `test/test.ts` for fulle xample

```ts
interface Person {
  id?: string;
  name: string;
  age: number;
  weight: number;
  favFoods: {
    asian: string;
    italian: string;
    american: string;
  };
}

const PersonYup = yup.object().shape({
  id: yup.string().required(),
  name: yup.string().required(),
  age: yup.number().required(),
  weight: yup.number().required(),
  favFoods: yup
    .object()
    .shape({ asian: yup.string().required(), italian: yup.string().required(), american: yup.string().required() })
    .required()
});

// Initialize Firebase
const firebaseConfig = {...};
firebase.initializeApp(firebaseConfig);

const batchRunner = new BatchRunner({
  fireStoreInstance: firebase.firestore(),
  firestoreModule: firebase.firestore
});

let personHelper = new FirestoreLift<Person>({
  collection: "person",
  fireStoreInstance: firebase.firestore(),
  batchRunner,
  yupSchema: PersonYup,
  addIdPropertyByDefault: true,
  prefixIdWithCollection: true
});
```


### Limitations

* Do not support sub-collections
* Do not support collection-groups
* Do not support array data types (they are stored as objects anyway)
