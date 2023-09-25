##Arrays and Objects

#Arrays

"let hobbies: string"   Single item 
 "let hobbies: string[];"  Square brackets used for multiple items in an array, also works with numbers

 "hobbies = ['Sports', 'Cooking'];"


 #Objects

TS allows "any" as a type
"let person: { 
  name: string;
  age: number;
 };  "

 person = {
  name: 'Max',
  age: 32
 };

To create an array of things, use [] after
 let people: { 
   name: string;
   age: number;
  }[];  