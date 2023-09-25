##Interfaces 

object type definition   interface-->implement

"interface Human {
  firstName: string;
  age: number;
  greet: () => void; //no parameters and returns nothing 
}

let max: Human;
max = {
  firstName: "Max",
  age: 32,
  greet () {
  console.log("Hello!");
  },
};

class Instructor implements Human {
  firstName: string;
  age: number;
  greet() {
    console.log('Hello!');
  };
}"

When coding as a team this can help insure the right parameters are followed for objects and classes