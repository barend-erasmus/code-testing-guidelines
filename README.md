# Code Testing Guidelines

Unit Test, Integration Test, Functional Test

## Unit Test

Unit Test should only isolated methods which do not have dependencies on any external resources.

If the method is using an external resources such as file system, network or database, it's not a unit test.

```javascript

// Method
function isValidDateFormat(dateString) {
    const pattern = new RegExp(/^\d{2}([./-])\d{2}\1\d{4}$/);
    return pattern.test(dateString);
}

// Unit Test
describe('isValidDateFormat', () => {
    it('should return false given invalid date format', () => {
        const result = isValidDateFormat('2017-01-01');
        expect(result).to.be.false;
    });

    it('should return true given valid date format', () => {
        const result = isValidDateFormat('01-01-2017');
        expect(result).to.be.true;
    });
});

```

## Integration Test

Integration Test should test the integration of your code with external resources. 

```javascript

// Method
function getCurrentDateFromServer() {
    return request('http://my-date-server.local/current-date')
        .then((result) => {
            return parseResulToDate(result);
        });
}

// This method should be Unit Tested
function parseResulToDate(result) {
    try {
        return JSON.parse(result).currentDate;
    }catch(err) {
        return null;
    }
}

// Integration Test
describe('getCurrentDateFromServer', () => {
    it('should return date', () => {
        return getCurrentDateFromServer().then((result) => {
            expect(result).to.be.not.null;
        });
    });
});

```

## Functional Test

Functional Test (referred to as E2E, Black Box or Browser Testing) should test at the highest level with as little knowledge as possible of the internal workings.

```javascript

// Method
function requestCurrentDate() {
    return getCurrentDateFromServer().then((result) => {
        if (isValidDateFormat(result)) {
            return result;
        }else {
            return null;
        }
    });
}

// Functional Test
describe('requestCurrentDate', () => {
    it('should return date', () => {
        return requestCurrentDate().then((result) => {
            expect(result).to.be.not.null;
        });
    });
});

```