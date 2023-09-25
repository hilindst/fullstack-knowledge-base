##Generics 

<> attached to function, helps TS infer what types are being used
any value can be exchanged for T in this instance.  Type script will recognized that the demoArray is a number and that the updatedArray will also be a number array

"function insertAtBeginning<T>(array: T[], value: T) {
  const newArray = [value, ...array];
  return newArray;
}

const demoArray = [1, 2, 3];

const updatedArray = insertAtBeginning(demoArray, -1); //[-1, 1, 2, 3,]"