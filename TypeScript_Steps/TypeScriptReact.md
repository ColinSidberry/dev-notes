### 1. React and TypeScript
```TS
npx create-react-app todo-ts-demo --template typescript

```

### 3. Props
```TS
interface StudentProps {
  first: string
  last: string
}

function PersonInfo ({ first, last }: StudentProps ) {

}

```

### 4. State
```TS
interface UserInterface {
  email: string;
  username: string;
  phone?: number;
}

const [firstName, setFirstName] = useState("")
const [user, setUser] = useState<UserInterface | null>(null)
const [data, setData] = useState<{info: string}>({info: "awesome!"})

```

### 6.Context
```TS
interface CompanyContextInterface {
  name: string;
  address: string;
}

const CompanyContext = React.createContext<CompanyContextInterface | null>(null);

// Provider in your app

const sampleContext: CompanyContextInterface = {
  name: "Rithm School",
  address: "500 Sansome Street",
};

```

### 7. Form/Events
```TS
function handleChange(evt: React.ChangeEvent<HTMLInputElement>) {
  const { name, value } = evt.target;
  setFormData(formData => ({
    ...formData,
    [name]: value,
  }));
}

/** Submit form: call function from parent & clear inputs. */
function handleSubmit(evt: React.FormEvent) {
  evt.preventDefault();
  // do stuff
}
```
### 5.UseEffect??
```TS

```
### Enum ?????
```TS

enum Level {
  Support,
  Moderator,
  Owner
};
```