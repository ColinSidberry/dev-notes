```js
// 1. Add to js file:
module.exports = { functionName }

// 2. Add to functionName.test.js file (mocking example)
"use strict";

const { functionName } = require("./functionName");

describe("testing countDown recursive function", () => {
    const log = console.log; // save original console.log function

    beforeEach(() => {
        console.log = jest.fn(); // create a new mock function for each test
    });

    afterAll(() => {
        console.log = log; // restore original console.log after all tests
    });

    it("counts down properly", () => {
        functionName(5);

        expect(console.log).toHaveBeenCalled();

        const consoleLogs = console.log.mock.calls;
        
        expect(consoleLogs[0][0]).toEqual(5);
        expect(consoleLogs[1][0]).toEqual(4);
        expect(consoleLogs[2][0]).toEqual(3);
        expect(consoleLogs[3][0]).toEqual(2);
        expect(consoleLogs[4][0]).toEqual(1);
        expect(consoleLogs[5][0]).toEqual(0);

        log(consoleLogs); // actually log out what the mock was called with
    });
});
```