### 1. Set Up
```TS
//1-------------------------------------
npm install @types/lodash --save-dev//lodash or type needed

//2-------------------------------------
npx webpack --watch
//vs
npx tsc typeScriptFile.ts
npx tsc --out output.js input.ts
npx tsc --init

//3 set tsconfig.json-----------------------------------
strict: false
```

### 2. Basic Variable
```TS
//string--------------------------------------------------
let variableName :string = value;

//array---------------------------------------------------
let badBabyNames: string[] = ["Carrot", "Chicken Nugget"];

//function------------------------------------------------
function isSeven(n: number): string {
  if (n === 7) return "THAT IS INDEED 7!";
  return "NOPE, NOT SEVEN :( ";
}
//function returns void------------------------------------
function hi(): void {
  console.log("HI"); 
}

```

### 3. Interfaces
```TS
//Declaration------------------------------------------------
interface IEmployee {
    name: string;
    title?: string;//optional parameter
    getSalary: () => number;
    getManagerInfo(managerId: number): IManager;
}

//Usage-----------------------------------------------------
const davis: IEmployee = {
    name: "Davis",
    title: "student",
    getSalary: function(){
        return 10
    },
    getManagerInfo(id:number){

    }
}
//Extensibility--------------------------------------------
interface IAdmin extends IEmployee {
  securityLevel: Level
}
```

### 5. Generics
```TS
//Function Declaration---------------------------------------
function identity<T>(arg: T): T {
  return arg;
}

//Function Instantiation--------------------------------------
identity<number>(10)
```

### AJAX/Promise
```TS
//With AJAX Call----------------------------------------------
async function getData(username: string): Promise<string> {
  const apiUrl = `https://api.github.com/users/${username}`;
  const res = await axios(apiUrl);
  const jsonResponse = await res.json();
  return jsonResponse.name;
}

```

### 6. Flexible Objects/Record
```TS
const instructorData: Record<string,string> = {};

instructorData.firstName = "Enzo";

```

### 7. Forcing Type w/ "as"
```TS
const el = document.querySelector("h1") as HTMLELement;

```
