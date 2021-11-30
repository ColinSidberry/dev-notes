## JS Jasmine Testing
  ### 1. Add dependency lines to HTML
  ```html
  <!-- 1. Add dependency lines to HTML -->
  <!doctype html>
  <html>
  <head>
  <title>Taxes Tests</title>

  <!-- 1a. include CSS for Jasmine -->
  <link rel="stylesheet"
    href="https://unpkg.com/jasmine-core/lib/jasmine-core/jasmine.css" />
  </head>
  <body>

  <!-- 1b. include JS for Jasmine -->  
  <script 
    src="https://unpkg.com/jasmine-core/lib/jasmine-core/jasmine.js"></script>
  <script 
    src="https://unpkg.com/jasmine-core/lib/jasmine-core/jasmine-html.js"></script>
  <script 
    src= "https://unpkg.com/jasmine-core/lib/jasmine-core/boot.js"></script>

  <!-- 1c. include your JS & test file -->
  <script src="taxes.js"></script> 
  <script src="taxes.test.js"></script>
  </body>
  </html>
  ```
  ### 2. Sample Jasmine Test
  ```js
  // Test
  it('should calculate lower-bracket taxes', function () {
    expect(calculateTaxes(10000)).toEqual(1500);
    expect(calculateTaxes(20000)).toEqual(3000);
  });

  // vs. code being tested
  function calculateTaxes(income) {
    if (income > 30000) {
      return income * 0.25;
    } else {
      return income * 0.15;
    }
  }

  console.log(calculateTaxes(500))
  ```
  ### 3. Common Matchers
  .toEqual(obj)
  .toBe(obj)
  .toContain(obj)
  .not.
  https://jasmine.github.io/api/edge/matchers.html 
  https://r23.students.rithmschool.com/lectures/js-intermediate-1/

## Debugging
```js
console.debug("drawLine", "x=", x, "y=", y); // provide context
console.warn("Yellow warning if something happens but not that important.")
console.error("Red if an error happens.")
// https://r23.students.rithmschool.com/lectures/pro-coding/ > "Rithm Code Requirements: JavaScript"
```

## JS Pieces (FIXME: Rename)

```js
// 0. making an js board
function makeBoard() { 
  for (let y = 0; y < HEIGHT; y++) {
    board.push(Array.from({ length: WIDTH }));
  }
} 
// https://github.com/ColinSidberry/reference-code/blob/main/JS_JQuery/0825-connect4/connect4.js

// Creating DOM Elements
const top = document.createElement('tr');
// https://github.com/ColinSidberry/reference-code/blob/main/JS_JQuery/0825-connect4/connect4.js

// Setting DOM Attributes
top.setAttribute('id', 'column-top');
// https://github.com/ColinSidberry/reference-code/blob/main/JS_JQuery/0825-connect4/connect4.js
```

## OnClick function
```html
<!-- 1. Form & Input ID's -->
<form id="seed-form">
  <div class="form-group m-3">
    <label for="title" class="mb-2">Title</label>
    <input id="seed-title" class="form-control" name="title" placeholder="Enter a title." />
  </div>
  <div class="form-group m-3">
    <label for="seed-text" class="mb-2">Seed Text</label>
    <input id="seed-text" class="form-control" name="seed-text" placeholder="Enter seed text." />
  </div>
</form>

<!-- 2. jQuery Script Tag -->
<script src="http://unpkg.com/jquery"></script>
```

```js
// 3. Get DOM Elements
const $seedForm = $("#seed-form"); //JS document.getElementById('seed-form');
const $seedTitle = $("#seed-title");
const $seedText = $("#seed-text");
const $markovOutput = $("markov-output")

// 4. Add Event Listener
$seedForm.on("submit", handleSeedSubmit); //$seedForm.addEventListener('click', handleSeedSubmit);

//5. Write handleSubmit function
function handleSeedSubmit(evt) {
    evt.preventDefault();

    const seedText = $seedText.val();
    const seedTitle = $seedTitle.val();

    const randomText = markovMachine.getText(50);

    markovOutput.append(`
        <div class="card m-3">
            <div class="card-body">
                <p>${randomText}</p>
            </div>
        </div>`
    );
};

$seedForm.on("submit", handleSeedSubmit);
```

## Event Delegation #FIXME
```TS
//1.HTML Setup
function populateShows(shows: Show[]) {
  $showsList.empty();
  for (let show of shows) {
    const $show = $(
      `<div data-3.RETURN-DATA="${show.id}" class="2.GRAB-ME">
            <button class="1.WHEN-FIRED">
            Episodes
            </button>
       </div>
      `
    );
    $showsList.append($show);
  }
}

//2. Add jquery script
<script src="http://unpkg.com/jquery"></script>

//2. Get DOM element
const $showsList = $("#showsList");

//2.Adding Event Listener
$showsList.on("click", ".1.WHEN-FIRED", getEpisodesAndDisplay);

//3.OnClick Function
async function getEpisodesAndDisplay(evt: JQuery.ClickEvent): Promise<void> {
  const showId = $(evt.target).closest(".2.GRAB-ME").data("3.RETURN-DATA");
  const episodes = await getEpisodesOfShow(showId);
  populateEpisodes(episodes);
}
"From tvmaze.ts"
```

## AXIOS Call
```HTML
<!-- 1. Add axios script tag -->
```
```JS
// 2. Axios call
async function getShowsByTerm(term) {
  const response = await axios({
    url: `${TVMAZE_API_URL}search/shows?q=${term}`,
    method: "GET",
    data: {}, // data vs. params when to use?
    params: {},
    headers: { Authorization: `Bearer 1222token2323` }
  });
```