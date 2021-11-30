### 8a. Testing (Routes.test.js) 
```JSX
import React from 'react';
import { MemoryRouter } from 'react-router-dom';
import { render, screen } from '@testing-library/react';
import Routes from './Routes';
import dogs from "./_testCommon"

// smoke test
it('renders without crashing', function() {
  render(
    <MemoryRouter>
      <Nav />
    </MemoryRouter>
  );
});

// snapshot test
it('matches snapshot', function() {
  const container = render(
    <MemoryRouter>
      <Nav />
    </MemoryRouter>
  );
  expect(container.asFragment()).toMatchSnapshot();
});

//Happy: Renders All
test('renders all dog names in the dog list', () => {
  const { getByText } = render(
    <MemoryRouter initialEntries={["/dogs"]}>
      <Routes dogs={dogs} />
    </MemoryRouter>
  );
  const dogNames = dogs.map(d => d.name);
  for (const name of dogNames) {
    const linkElement = getByText(new RegExp(name, "i"));
    expect(linkElement).toBeInTheDocument();
  }
});

//Happy: Renders Correct Info & only what is expected
test('renders only test\'s info', () => {
  const { getByText } = render(
    <MemoryRouter initialEntries={["/dogs/test"]}>
      <Routes dogs={dogs} />
    </MemoryRouter>
  );
  const testInfo = dogs.find(d => d.name === "test");
  const testInfo2 = dogs.find(d => d.name === "mocked-dog");

  const liElement = getByText(new RegExp(testInfo.facts[0], "i"));
  expect(liElement).toBeInTheDocument();

  expect(screen.queryByText(new RegExp(testInfo2.facts[0], "i"))).toBeNull();
});

//Unhappy: Bad Specific Route
test('renders the home page on a dog that is not found', () => {
  const { getByText } = render(
    <MemoryRouter initialEntries={["/dogs/wrong"]}>
      <Routes dogs={dogs} />
    </MemoryRouter>
  );

  expect(getByText("HELLO. WE HAVE DOGS. CLICK ON THEM FOR MORE INFO.")).toBeInTheDocument()

  const dogNames = dogs.map(d => d.name);
  for (const name of dogNames) {
    const linkElement = getByText(new RegExp(name, "i"));
    expect(linkElement).toBeInTheDocument();
  }
});

//Unhappy: Bad Base Route
test('renders home on a bad route', () => {
  const { getByText } = render(
    <MemoryRouter initialEntries={["/bad-route"]}>
      <Routes dogs={dogs} />
    </MemoryRouter>
  );
  const dogNames = dogs.map(d => d.name);
  for (const name of dogNames) {
    const linkElement = getByText(new RegExp(name, "i")); //Question: Why the link element?
    expect(linkElement).toBeInTheDocument();
  }
});

```

### 8b. Testing: Page with params (mocking params) -> use memoryRouter format instead
```JSX
import FilterDogDetails from './FilterDogDetails';
import { render } from "@testing-library/react"
import { MemoryRouter } from "react-router-dom"

//Unhappy: Explicitly testing invalid params (mocking params)
test("it returns null with empty params", function () {

  jest.mock('react-router-dom', () => ({
    useParams: jest.fn().mockReturnValue({}),
  }))

  const { container } = render(
    <MemoryRouter>
      <FilterDogDetails />
    </MemoryRouter>)

  expect(container).toBeEmptyDOMElement()
})
```

### 8c. Testing: Axios call in component (mocking axios)
```JSX
import { render, screen, waitFor } from '@testing-library/react';
import App from './App';
import axios from "axios"
import testDogsData from "./_testCommon"

axios.get = jest.fn();

//Happy: axios call in component (mocking axios)
test('renders learn react link', async () => {
  axios.get.mockResolvedValue({ data: testDogsData });
  const { container } = render(<App />);
  const heading = await waitFor(() => screen.findByText('Welcome!'));
  const nav = container.querySelector("nav")
  expect(nav).toBeInTheDocument()

  expect(heading).toBeInTheDocument();
  expect(axios.get).toHaveBeenCalled()
});
```

### 8d. Testing: Mocking a function
```JSX
import * as random from "./random";

random.choice = jest.fn();

import { getRandomCard, getPair, scoreHand, score } from "./nineteen";


test("getRandomCard", function () {
  random.choice
      .mockReturnValueOnce("9")
      .mockReturnValueOnce("C");
  const card = getRandomCard();
  expect(card).toEqual({ rank: "9", suit: "C" });
});
```

### 8e. Testing Form Inputs
```JSX
import React from "react";
import { render, fireEvent } from "@testing-library/react";

import ShoppingList from "./ShoppingList";


it("renders without crashing", function () {
  render(<ShoppingList />);
});


it("matches snapshot", function () {
  const { asFragment } = render(<ShoppingList />);
  expect(asFragment()).toMatchSnapshot();
});


//Note: Testing form input
it("can add a new item", function () {
  const { getByLabelText, queryByText } = render(<ShoppingList />);

  // no items yet
  expect(queryByText("ice cream: 100")).not.toBeInTheDocument();

  const nameInput = getByLabelText("Name:");
  const qtyInput = getByLabelText("Qty:");
  const submitBtn = queryByText("Add a new item!");

  // fill out the form
  fireEvent.change(nameInput, { target: { value: "ice cream" } });
  fireEvent.change(qtyInput, { target: { value: 100 } });
  fireEvent.click(submitBtn);

  // item exists!
  expect(queryByText("ice cream: 100")).toBeInTheDocument();
});

```